<script lang="ts">
	import { onMount, onDestroy } from 'svelte';
	import { Conversation } from '@elevenlabs/client';
	import { PUBLIC_ELEVENLABS_AGENT_ID } from '$env/static/public';

	type Status = 'idle' | 'connecting' | 'connected';
	type Mode = 'listening' | 'speaking';

	let conversation: Awaited<ReturnType<typeof Conversation.startSession>> | null = null;
	let status = $state<Status>('idle');
	let mode = $state<Mode>('listening');
	let error = $state<string | null>(null);
	let callDuration = $state(0);
	let callTimer: ReturnType<typeof setInterval> | null = null;

	// Audio visualization
	let audioContext: AudioContext | null = null;
	let analyser: AnalyserNode | null = null;
	let audioLevel = $state(0);
	let animationFrame: number | null = null;

	// Ambient restaurant audio (phone quality)
	let ambientAudio: HTMLAudioElement | null = null;
	const AMBIENT_VOLUME = 0.08;

	function formatTime(seconds: number): string {
		const m = Math.floor(seconds / 60);
		const s = seconds % 60;
		return `${m}:${s.toString().padStart(2, '0')}`;
	}

	function startCallTimer() {
		callDuration = 0;
		callTimer = setInterval(() => {
			callDuration++;
		}, 1000);
	}

	function stopCallTimer() {
		if (callTimer) {
			clearInterval(callTimer);
			callTimer = null;
		}
		callDuration = 0;
	}

	function startAudioVisualization(stream: MediaStream) {
		audioContext = new AudioContext();
		analyser = audioContext.createAnalyser();
		analyser.fftSize = 256;
		analyser.smoothingTimeConstant = 0.8;

		const source = audioContext.createMediaStreamSource(stream);
		source.connect(analyser);

		const dataArray = new Uint8Array(analyser.frequencyBinCount);

		function updateLevel() {
			if (!analyser) return;
			analyser.getByteFrequencyData(dataArray);
			// Get average of lower frequencies (voice range)
			const slice = dataArray.slice(0, 32);
			const avg = slice.reduce((a, b) => a + b, 0) / slice.length;
			audioLevel = avg / 255;
			animationFrame = requestAnimationFrame(updateLevel);
		}
		updateLevel();
	}

	function stopAudioVisualization() {
		if (animationFrame) {
			cancelAnimationFrame(animationFrame);
			animationFrame = null;
		}
		if (audioContext) {
			audioContext.close();
			audioContext = null;
		}
		analyser = null;
		audioLevel = 0;
	}

	function startAmbientAudio() {
		if (!ambientAudio) {
			ambientAudio = new Audio('/audio/restaurant-ambience-phone.mp3');
			ambientAudio.loop = true;
			ambientAudio.volume = 0;
		}
		ambientAudio.play();
		let vol = 0;
		const fadeIn = setInterval(() => {
			vol += 0.015;
			if (vol >= AMBIENT_VOLUME) {
				ambientAudio!.volume = AMBIENT_VOLUME;
				clearInterval(fadeIn);
			} else {
				ambientAudio!.volume = vol;
			}
		}, 30);
	}

	function stopAmbientAudio() {
		if (!ambientAudio) return;
		const fadeOut = setInterval(() => {
			if (ambientAudio!.volume > 0.01) {
				ambientAudio!.volume -= 0.015;
			} else {
				ambientAudio!.pause();
				ambientAudio!.currentTime = 0;
				ambientAudio!.volume = 0;
				clearInterval(fadeOut);
			}
		}, 30);
	}

	const agentId = PUBLIC_ELEVENLABS_AGENT_ID;

	async function toggleCall() {
		if (status === 'connected') {
			await endCall();
		} else {
			await startCall();
		}
	}

	async function startCall() {
		if (!agentId || agentId === 'your_agent_id_here') {
			error = 'Configure agent ID in .env';
			return;
		}

		error = null;
		status = 'connecting';

		try {
			const stream = await navigator.mediaDevices.getUserMedia({ audio: true });
			startAudioVisualization(stream);

			conversation = await Conversation.startSession({
				agentId,
				onConnect: () => {
					status = 'connected';
					startAmbientAudio();
					startCallTimer();
				},
				onDisconnect: () => {
					status = 'idle';
					mode = 'listening';
					stopAmbientAudio();
					stopCallTimer();
					stopAudioVisualization();
				},
				onError: (err) => {
					console.error('Error:', err);
					error = 'Connection failed';
					status = 'idle';
					stopAmbientAudio();
					stopCallTimer();
					stopAudioVisualization();
				},
				onModeChange: (data) => {
					mode = data.mode === 'speaking' ? 'speaking' : 'listening';
				}
			});
		} catch (err) {
			console.error('Failed:', err);
			error = 'Microphone access required';
			status = 'idle';
			stopAudioVisualization();
		}
	}

	async function endCall() {
		if (conversation) {
			await conversation.endSession();
			conversation = null;
		}
		stopAmbientAudio();
		stopCallTimer();
		stopAudioVisualization();
		status = 'idle';
	}

	onDestroy(() => {
		if (conversation) conversation.endSession();
		if (ambientAudio) {
			ambientAudio.pause();
			ambientAudio = null;
		}
		stopCallTimer();
		stopAudioVisualization();
	});
</script>

<div class="relative">
	<!-- Phone call container -->
	<button
		onclick={toggleCall}
		disabled={status === 'connecting'}
		class="group relative flex flex-col items-center gap-5 focus:outline-none"
		aria-label={status === 'connected' ? 'End call' : 'Start demo call'}
	>
		<!-- Outer glow container -->
		<div class="relative">
			<!-- Deep ambient glow -->
			<div
				class="absolute inset-0 rounded-full blur-3xl transition-all duration-1000
					{status === 'connected'
						? (mode === 'speaking' ? 'opacity-80 scale-[3]' : 'opacity-50 scale-[2.5]')
						: 'opacity-20 scale-[2] group-hover:opacity-40'}"
				style="background: var(--color-coral);"
			></div>

			<!-- Mid glow layer -->
			<div
				class="absolute inset-0 rounded-full blur-2xl transition-all duration-700
					{status === 'connected'
						? (mode === 'speaking' ? 'opacity-90 scale-[2]' : 'opacity-60 scale-[1.7]')
						: 'opacity-30 scale-[1.5] group-hover:opacity-50'}"
				style="background: var(--color-coral);"
			></div>

			<!-- Audio-reactive glow (when listening) -->
			{#if status === 'connected' && mode === 'listening'}
				<div
					class="absolute inset-0 rounded-full blur-xl transition-opacity duration-150"
					style="background: var(--color-coral); transform: scale({1.3 + audioLevel * 0.8}); opacity: {0.3 + audioLevel * 0.5};"
				></div>
			{/if}

			<!-- Attention pulse ring - only when idle -->
			{#if status === 'idle'}
				<div class="absolute inset-0 rounded-full border-2 border-coral/40 animate-attention"></div>
			{/if}

			<!-- Connecting dial animation -->
			{#if status === 'connecting'}
				<div class="absolute inset-0 rounded-full border-2 border-coral/60 animate-dial"></div>
				<div class="absolute inset-0 rounded-full border-2 border-coral/40 animate-dial delay-200"></div>
			{/if}

			<!-- Speaking wave rings -->
			{#if status === 'connected' && mode === 'speaking'}
				<div class="absolute inset-0 rounded-full border-2 border-coral/50 animate-wave"></div>
				<div class="absolute inset-0 rounded-full border-2 border-coral/30 animate-wave delay-300"></div>
				<div class="absolute inset-0 rounded-full border-2 border-coral/20 animate-wave delay-600"></div>
			{/if}

			<!-- Main orb -->
			<div
				class="phone-orb relative w-32 h-32 md:w-40 md:h-40 lg:w-48 lg:h-48 rounded-full flex items-center justify-center transition-all duration-500 overflow-hidden
					{status === 'connected'
						? 'bg-coral shadow-[0_0_120px_rgba(232,93,64,0.7)]'
						: status === 'connecting'
							? 'bg-coral/80'
							: 'bg-coral/90 group-hover:bg-coral shadow-[0_0_60px_rgba(232,93,64,0.3)] group-hover:shadow-[0_0_100px_rgba(232,93,64,0.5)]'}"
			>
				<!-- Scan lines overlay (phone/CRT effect) -->
				{#if status === 'connected'}
					<div class="absolute inset-0 scanlines pointer-events-none"></div>
					<div class="absolute inset-0 noise pointer-events-none"></div>
				{/if}

				<!-- Inner ring -->
				{#if status === 'connected'}
					<div
						class="absolute inset-3 md:inset-4 rounded-full border-2 transition-all duration-300
							{mode === 'speaking' ? 'border-cream/70 scale-105 animate-breathe' : 'border-cream/40 scale-95'}"
					></div>
				{/if}

				<!-- Audio level ring (listening mode) -->
				{#if status === 'connected' && mode === 'listening'}
					<div
						class="absolute inset-2 md:inset-3 rounded-full border transition-all duration-100 border-cream/30"
						style="transform: scale({0.9 + audioLevel * 0.15});"
					></div>
				{/if}

				<!-- Icon -->
				{#if status === 'connected'}
					<!-- Phone icon when connected -->
					<svg class="w-10 h-10 md:w-12 md:h-12 lg:w-14 lg:h-14 text-cream drop-shadow-lg relative z-10 {mode === 'speaking' ? 'animate-subtle-pulse' : ''}" fill="currentColor" viewBox="0 0 24 24">
						<path d="M6.62 10.79c1.44 2.83 3.76 5.14 6.59 6.59l2.2-2.2c.27-.27.67-.36 1.02-.24 1.12.37 2.33.57 3.57.57.55 0 1 .45 1 1V20c0 .55-.45 1-1 1-9.39 0-17-7.61-17-17 0-.55.45-1 1-1h3.5c.55 0 1 .45 1 1 0 1.25.2 2.45.57 3.57.11.35.03.74-.25 1.02l-2.2 2.2z"/>
					</svg>
				{:else if status === 'connecting'}
					<!-- Dialing dots -->
					<div class="flex gap-2">
						<div class="w-2 h-2 md:w-3 md:h-3 rounded-full bg-cream animate-dial-dot"></div>
						<div class="w-2 h-2 md:w-3 md:h-3 rounded-full bg-cream animate-dial-dot delay-200"></div>
						<div class="w-2 h-2 md:w-3 md:h-3 rounded-full bg-cream animate-dial-dot delay-400"></div>
					</div>
				{:else}
					<!-- Mic icon when idle -->
					<svg class="w-10 h-10 md:w-12 md:h-12 lg:w-14 lg:h-14 text-cream transition-transform group-hover:scale-110 drop-shadow-lg" fill="currentColor" viewBox="0 0 24 24">
						<path d="M12 14c1.66 0 3-1.34 3-3V5c0-1.66-1.34-3-3-3S9 3.34 9 5v6c0 1.66 1.34 3 3 3z"/>
						<path d="M17 11c0 2.76-2.24 5-5 5s-5-2.24-5-5H5c0 3.53 2.61 6.43 6 6.92V21h2v-3.08c3.39-.49 6-3.39 6-6.92h-2z"/>
					</svg>
				{/if}
			</div>
		</div>

		<!-- Status display -->
		<div class="text-center min-h-[60px] flex flex-col justify-center">
			{#if error}
				<p class="text-coral text-base font-medium">{error}</p>
			{:else if status === 'connected'}
				<!-- Connected state with call info -->
				<div class="mb-1">
					<span class="text-cream/60 text-xs font-mono">{formatTime(callDuration)}</span>
				</div>
				<p class="text-cream text-base md:text-lg font-display">
					{mode === 'speaking' ? 'Jessica' : 'Listening...'}
				</p>
				<p class="text-cream/50 text-xs tracking-wide">tap to end call</p>
			{:else if status === 'connecting'}
				<p class="text-warm-400 text-base animate-pulse">Dialing...</p>
				<p class="text-warm-400/60 text-xs">Casa Luna</p>
			{:else}
				<p class="text-cream text-base md:text-lg font-display">Try it</p>
				<p class="text-cream/60 text-sm">Call Casa Luna</p>
			{/if}
		</div>
	</button>
</div>

<style>
	/* Scan lines effect */
	.scanlines {
		background: repeating-linear-gradient(
			0deg,
			rgba(0, 0, 0, 0.1) 0px,
			rgba(0, 0, 0, 0.1) 1px,
			transparent 1px,
			transparent 3px
		);
		animation: scanline-move 8s linear infinite;
	}

	@keyframes scanline-move {
		0% { background-position: 0 0; }
		100% { background-position: 0 100px; }
	}

	/* Subtle noise texture */
	.noise {
		background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 200 200' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='noise'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23noise)'/%3E%3C/svg%3E");
		opacity: 0.03;
	}

	/* Attention pulse (idle state) */
	@keyframes attention {
		0%, 100% {
			transform: scale(1);
			opacity: 0.5;
		}
		50% {
			transform: scale(1.4);
			opacity: 0;
		}
	}
	.animate-attention {
		animation: attention 2.5s ease-in-out infinite;
	}

	/* Dialing animation */
	@keyframes dial {
		0% {
			transform: scale(1);
			opacity: 0.6;
		}
		100% {
			transform: scale(1.8);
			opacity: 0;
		}
	}
	.animate-dial {
		animation: dial 1.2s ease-out infinite;
	}

	/* Dialing dots */
	@keyframes dial-dot {
		0%, 100% { opacity: 0.3; transform: scale(0.8); }
		50% { opacity: 1; transform: scale(1.2); }
	}
	.animate-dial-dot {
		animation: dial-dot 1s ease-in-out infinite;
	}

	/* Speaking wave animation */
	@keyframes wave {
		0% {
			transform: scale(1);
			opacity: 0.5;
		}
		100% {
			transform: scale(2);
			opacity: 0;
		}
	}
	.animate-wave {
		animation: wave 1.5s ease-out infinite;
	}

	/* Breathing effect for inner ring */
	@keyframes breathe {
		0%, 100% { transform: scale(1.05); opacity: 0.7; }
		50% { transform: scale(1.1); opacity: 0.9; }
	}
	.animate-breathe {
		animation: breathe 1s ease-in-out infinite;
	}

	/* Subtle icon pulse */
	@keyframes subtle-pulse {
		0%, 100% { transform: scale(1); }
		50% { transform: scale(1.05); }
	}
	.animate-subtle-pulse {
		animation: subtle-pulse 0.8s ease-in-out infinite;
	}

	/* Signal bars */
	@keyframes signal {
		0%, 100% { opacity: 1; }
		50% { opacity: 0.5; }
	}
	.animate-signal {
		animation: signal 2s ease-in-out infinite;
	}

	/* Delay utilities */
	.delay-100 { animation-delay: 0.1s; }
	.delay-200 { animation-delay: 0.2s; }
	.delay-300 { animation-delay: 0.3s; }
	.delay-400 { animation-delay: 0.4s; }
	.delay-600 { animation-delay: 0.6s; }
</style>
