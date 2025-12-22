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

	// Ambient restaurant audio
	let ambientAudio: HTMLAudioElement | null = null;
	const AMBIENT_VOLUME = 0.15; // Subtle background level

	function startAmbientAudio() {
		if (!ambientAudio) {
			ambientAudio = new Audio('/audio/restaurant-ambience.mp3');
			ambientAudio.loop = true;
			ambientAudio.volume = 0;
		}
		ambientAudio.play();
		// Fade in
		let vol = 0;
		const fadeIn = setInterval(() => {
			vol += 0.01;
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
		// Fade out
		const fadeOut = setInterval(() => {
			if (ambientAudio!.volume > 0.01) {
				ambientAudio!.volume -= 0.01;
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
			await navigator.mediaDevices.getUserMedia({ audio: true });

			conversation = await Conversation.startSession({
				agentId,
				onConnect: () => {
					status = 'connected';
					startAmbientAudio();
				},
				onDisconnect: () => {
					status = 'idle';
					mode = 'listening';
					stopAmbientAudio();
				},
				onError: (err) => {
					console.error('Error:', err);
					error = 'Connection failed';
					status = 'idle';
					stopAmbientAudio();
				},
				onModeChange: (data) => {
					mode = data.mode === 'speaking' ? 'speaking' : 'listening';
				}
			});
		} catch (err) {
			console.error('Failed:', err);
			error = 'Microphone access required';
			status = 'idle';
		}
	}

	async function endCall() {
		if (conversation) {
			await conversation.endSession();
			conversation = null;
		}
		stopAmbientAudio();
		status = 'idle';
	}

	onDestroy(() => {
		if (conversation) conversation.endSession();
		if (ambientAudio) {
			ambientAudio.pause();
			ambientAudio = null;
		}
	});
</script>

<!-- Glowing orb widget with attention pulse -->
<button
	onclick={toggleCall}
	disabled={status === 'connecting'}
	class="group relative flex flex-col items-center gap-6 focus:outline-none"
	aria-label={status === 'connected' ? 'End call' : 'Start demo call'}
>
	<!-- Glow layers -->
	<div class="relative">
		<!-- Ambient glow - always present, stronger when connected -->
		<div
			class="absolute inset-0 rounded-full blur-3xl transition-all duration-700
				{status === 'connected'
					? (mode === 'speaking' ? 'opacity-70' : 'opacity-40')
					: 'opacity-25 group-hover:opacity-50'}"
			style="background: var(--color-coral); transform: scale(2.5);"
		></div>
		<div
			class="absolute inset-0 rounded-full blur-2xl transition-all duration-500
				{status === 'connected'
					? (mode === 'speaking' ? 'opacity-80' : 'opacity-50')
					: 'opacity-35 group-hover:opacity-60'}"
			style="background: var(--color-coral); transform: scale(1.8);"
		></div>

		<!-- Attention pulse ring - only when idle -->
		{#if status === 'idle'}
			<div class="absolute inset-0 rounded-full border-2 border-coral/40 animate-attention"></div>
		{/if}

		<!-- Main orb -->
		<div
			class="relative w-32 h-32 md:w-40 md:h-40 lg:w-48 lg:h-48 rounded-full flex items-center justify-center transition-all duration-500
				{status === 'connected'
					? 'bg-coral shadow-[0_0_120px_rgba(232,93,64,0.6)]'
					: status === 'connecting'
						? 'bg-warm-400 animate-pulse'
						: 'bg-coral/90 group-hover:bg-coral shadow-[0_0_60px_rgba(232,93,64,0.3)] group-hover:shadow-[0_0_100px_rgba(232,93,64,0.5)]'}"
		>
			<!-- Inner ring -->
			{#if status === 'connected'}
				<div
					class="absolute inset-3 md:inset-4 rounded-full border-2 transition-all duration-300
						{mode === 'speaking' ? 'border-cream/60 scale-100' : 'border-cream/30 scale-95'}"
				></div>
			{/if}

			<!-- Icon -->
			{#if status === 'connected'}
				<!-- Phone icon when connected -->
				<svg class="w-10 h-10 md:w-12 md:h-12 lg:w-14 lg:h-14 text-cream" fill="currentColor" viewBox="0 0 24 24">
					<path d="M6.62 10.79c1.44 2.83 3.76 5.14 6.59 6.59l2.2-2.2c.27-.27.67-.36 1.02-.24 1.12.37 2.33.57 3.57.57.55 0 1 .45 1 1V20c0 .55-.45 1-1 1-9.39 0-17-7.61-17-17 0-.55.45-1 1-1h3.5c.55 0 1 .45 1 1 0 1.25.2 2.45.57 3.57.11.35.03.74-.25 1.02l-2.2 2.2z"/>
				</svg>
			{:else}
				<!-- Mic icon when idle -->
				<svg class="w-10 h-10 md:w-12 md:h-12 lg:w-14 lg:h-14 text-cream transition-transform group-hover:scale-110" fill="currentColor" viewBox="0 0 24 24">
					<path d="M12 14c1.66 0 3-1.34 3-3V5c0-1.66-1.34-3-3-3S9 3.34 9 5v6c0 1.66 1.34 3 3 3z"/>
					<path d="M17 11c0 2.76-2.24 5-5 5s-5-2.24-5-5H5c0 3.53 2.61 6.43 6 6.92V21h2v-3.08c3.39-.49 6-3.39 6-6.92h-2z"/>
				</svg>
			{/if}
		</div>

		<!-- Pulse rings when speaking -->
		{#if status === 'connected' && mode === 'speaking'}
			<div class="absolute inset-0 rounded-full border-2 border-coral animate-ping opacity-30"></div>
		{/if}
	</div>

	<!-- Label -->
	<div class="text-center h-12 flex flex-col justify-center">
		{#if error}
			<p class="text-coral text-base">{error}</p>
		{:else if status === 'connected'}
			<p class="text-cream text-base md:text-lg font-display">
				{mode === 'speaking' ? 'Jessica' : 'Listening...'}
			</p>
			<p class="text-cream/60 text-sm">tap to end</p>
		{:else if status === 'connecting'}
			<p class="text-warm-400 text-base">Connecting...</p>
		{:else}
			<p class="text-cream text-base md:text-lg font-display">Try it</p>
			<p class="text-cream/60 text-sm">Casa Luna</p>
		{/if}
	</div>
</button>

<style>
	@keyframes ping {
		75%, 100% {
			transform: scale(1.5);
			opacity: 0;
		}
	}
	.animate-ping {
		animation: ping 1.5s cubic-bezier(0, 0, 0.2, 1) infinite;
	}
	@keyframes attention {
		0%, 100% {
			transform: scale(1);
			opacity: 0.4;
		}
		50% {
			transform: scale(1.3);
			opacity: 0;
		}
	}
	.animate-attention {
		animation: attention 2.5s ease-in-out infinite;
	}
</style>
