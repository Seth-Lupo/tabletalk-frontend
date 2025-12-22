<script lang="ts">
	let mobileMenuOpen = $state(false);
</script>

<svelte:head>
	<title>Table Talk - Never Miss a Call Again</title>
	<meta name="description" content="AI-powered voice agent for restaurants. Handle reservations, answer menu questions, and take orders 24/7." />
</svelte:head>

<!-- Navigation -->
<nav class="fixed top-0 left-0 right-0 z-50 bg-cream/95 backdrop-blur-sm border-b border-warm-gray-200">
	<div class="max-w-6xl mx-auto px-6 py-4">
		<div class="flex items-center justify-between">
			<a href="/" class="font-display text-2xl font-semibold tracking-tight text-charcoal">
				Table Talk
			</a>

			<!-- Desktop Navigation -->
			<div class="hidden md:flex items-center gap-8">
				<a href="#product" class="text-warm-gray-600 hover:text-charcoal transition-colors font-body">Product</a>
				<a href="#pricing" class="text-warm-gray-600 hover:text-charcoal transition-colors font-body">Pricing</a>
				<a href="#contact" class="text-warm-gray-600 hover:text-charcoal transition-colors font-body">Contact</a>
				<a href="#contact" class="btn-primary">Get Started</a>
			</div>

			<!-- Mobile Menu Button -->
			<button
				class="md:hidden p-2"
				onclick={() => mobileMenuOpen = !mobileMenuOpen}
				aria-label="Toggle menu"
			>
				<svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
					{#if mobileMenuOpen}
						<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
					{:else}
						<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16M4 18h16" />
					{/if}
				</svg>
			</button>
		</div>

		<!-- Mobile Menu -->
		{#if mobileMenuOpen}
			<div class="md:hidden pt-4 pb-2 border-t border-warm-gray-200 mt-4">
				<div class="flex flex-col gap-4">
					<a href="#product" class="text-warm-gray-600 hover:text-charcoal transition-colors" onclick={() => mobileMenuOpen = false}>Product</a>
					<a href="#pricing" class="text-warm-gray-600 hover:text-charcoal transition-colors" onclick={() => mobileMenuOpen = false}>Pricing</a>
					<a href="#contact" class="text-warm-gray-600 hover:text-charcoal transition-colors" onclick={() => mobileMenuOpen = false}>Contact</a>
					<a href="#contact" class="btn-primary text-center" onclick={() => mobileMenuOpen = false}>Get Started</a>
				</div>
			</div>
		{/if}
	</div>
</nav>

<!-- Hero Section -->
<section class="min-h-screen flex items-center justify-center texture-overlay grid-texture pt-20">
	<div class="max-w-4xl mx-auto px-6 py-24 text-center">
		<div class="animate-fade-in-up opacity-0">
			<p class="font-display text-warm-gray-500 tracking-widest uppercase text-sm mb-6">
				For Restaurants That Care
			</p>
		</div>

		<h1 class="font-display text-5xl md:text-7xl lg:text-8xl font-medium text-charcoal mb-8 animate-fade-in-up opacity-0 animation-delay-100">
			Never Miss<br />
			<span class="italic">a Call</span> Again
		</h1>

		<div class="decorative-line mx-auto mb-8 animate-fade-in-up opacity-0 animation-delay-200"></div>

		<p class="font-body text-xl md:text-2xl text-warm-gray-600 max-w-2xl mx-auto mb-12 animate-fade-in-up opacity-0 animation-delay-300">
			Your AI-powered voice agent handles reservations, answers questions, and takes orders while you focus on what matters most — your guests.
		</p>

		<div class="flex flex-col sm:flex-row gap-4 justify-center animate-fade-in-up opacity-0 animation-delay-400">
			<a href="#contact" class="btn-primary">Start Your Free Trial</a>
			<a href="#product" class="btn-secondary">See How It Works</a>
		</div>

		<!-- 80s Style Decorative Element -->
		<div class="mt-20 animate-fade-in-up opacity-0 animation-delay-500">
			<div class="flex items-center justify-center gap-4 text-warm-gray-400">
				<div class="w-12 h-px bg-warm-gray-300"></div>
				<svg class="w-6 h-6" fill="currentColor" viewBox="0 0 24 24">
					<path d="M6.62 10.79c1.44 2.83 3.76 5.14 6.59 6.59l2.2-2.2c.27-.27.67-.36 1.02-.24 1.12.37 2.33.57 3.57.57.55 0 1 .45 1 1V20c0 .55-.45 1-1 1-9.39 0-17-7.61-17-17 0-.55.45-1 1-1h3.5c.55 0 1 .45 1 1 0 1.25.2 2.45.57 3.57.11.35.03.74-.25 1.02l-2.2 2.2z"/>
				</svg>
				<div class="w-12 h-px bg-warm-gray-300"></div>
			</div>
		</div>
	</div>
</section>

<!-- Product Section -->
<section id="product" class="py-24 md:py-32 bg-paper halftone">
	<div class="max-w-6xl mx-auto px-6">
		<div class="text-center mb-16">
			<p class="font-display text-warm-gray-500 tracking-widest uppercase text-sm mb-4">
				The Product
			</p>
			<h2 class="font-display text-4xl md:text-5xl font-medium text-charcoal mb-6">
				Your 24/7 Voice Concierge
			</h2>
			<div class="decorative-line mx-auto mb-6"></div>
			<p class="font-body text-lg text-warm-gray-600 max-w-2xl mx-auto">
				A warm, friendly voice that knows your restaurant inside and out — from your daily specials to every ingredient in every dish.
			</p>
		</div>

		<!-- Features Grid -->
		<div class="grid md:grid-cols-2 lg:grid-cols-3 gap-8">
			<!-- Feature 1: Reservations -->
			<div class="paper-texture p-8 border border-warm-gray-200 hover:border-warm-gray-300 transition-colors">
				<div class="w-12 h-12 bg-warm-gray-100 rounded-full flex items-center justify-center mb-6">
					<svg class="w-6 h-6 text-warm-gray-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
						<path stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" d="M8 7V3m8 4V3m-9 8h10M5 21h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v12a2 2 0 002 2z" />
					</svg>
				</div>
				<h3 class="font-display text-xl font-medium text-charcoal mb-3">
					Seamless Reservations
				</h3>
				<p class="font-body text-warm-gray-600">
					Connects directly to OpenTable, Resy, or your existing system. Books tables, confirms parties, and handles special requests with grace.
				</p>
			</div>

			<!-- Feature 2: Menu & Allergens -->
			<div class="paper-texture p-8 border border-warm-gray-200 hover:border-warm-gray-300 transition-colors">
				<div class="w-12 h-12 bg-warm-gray-100 rounded-full flex items-center justify-center mb-6">
					<svg class="w-6 h-6 text-warm-gray-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
						<path stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" d="M9 5H7a2 2 0 00-2 2v12a2 2 0 002 2h10a2 2 0 002-2V7a2 2 0 00-2-2h-2M9 5a2 2 0 002 2h2a2 2 0 002-2M9 5a2 2 0 012-2h2a2 2 0 012 2m-3 7h3m-3 4h3m-6-4h.01M9 16h.01" />
					</svg>
				</div>
				<h3 class="font-display text-xl font-medium text-charcoal mb-3">
					Menu Knowledge
				</h3>
				<p class="font-body text-warm-gray-600">
					Every dish, every ingredient, every allergen. Your voice agent answers dietary questions with confidence and care.
				</p>
			</div>

			<!-- Feature 3: Food Orders -->
			<div class="paper-texture p-8 border border-warm-gray-200 hover:border-warm-gray-300 transition-colors">
				<div class="w-12 h-12 bg-warm-gray-100 rounded-full flex items-center justify-center mb-6">
					<svg class="w-6 h-6 text-warm-gray-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
						<path stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" d="M16 11V7a4 4 0 00-8 0v4M5 9h14l1 12H4L5 9z" />
					</svg>
				</div>
				<h3 class="font-display text-xl font-medium text-charcoal mb-3">
					Takeout & Delivery
				</h3>
				<p class="font-body text-warm-gray-600">
					Takes orders over the phone, confirms details, and sends them straight to your kitchen. No third-party fees eating your margins.
				</p>
			</div>

			<!-- Feature 4: Hours & Info -->
			<div class="paper-texture p-8 border border-warm-gray-200 hover:border-warm-gray-300 transition-colors">
				<div class="w-12 h-12 bg-warm-gray-100 rounded-full flex items-center justify-center mb-6">
					<svg class="w-6 h-6 text-warm-gray-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
						<path stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" d="M12 8v4l3 3m6-3a9 9 0 11-18 0 9 9 0 0118 0z" />
					</svg>
				</div>
				<h3 class="font-display text-xl font-medium text-charcoal mb-3">
					Always Informed
				</h3>
				<p class="font-body text-warm-gray-600">
					Hours, location, parking, private dining — every question answered instantly while your staff stays focused on service.
				</p>
			</div>

			<!-- Feature 5: Natural Conversation -->
			<div class="paper-texture p-8 border border-warm-gray-200 hover:border-warm-gray-300 transition-colors">
				<div class="w-12 h-12 bg-warm-gray-100 rounded-full flex items-center justify-center mb-6">
					<svg class="w-6 h-6 text-warm-gray-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
						<path stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" d="M8 12h.01M12 12h.01M16 12h.01M21 12c0 4.418-4.03 8-9 8a9.863 9.863 0 01-4.255-.949L3 20l1.395-3.72C3.512 15.042 3 13.574 3 12c0-4.418 4.03-8 9-8s9 3.582 9 8z" />
					</svg>
				</div>
				<h3 class="font-display text-xl font-medium text-charcoal mb-3">
					Naturally Human
				</h3>
				<p class="font-body text-warm-gray-600">
					No robotic menus or "press 1 for..." — just warm, natural conversation that reflects your restaurant's personality.
				</p>
			</div>

			<!-- Feature 6: Handoff -->
			<div class="paper-texture p-8 border border-warm-gray-200 hover:border-warm-gray-300 transition-colors">
				<div class="w-12 h-12 bg-warm-gray-100 rounded-full flex items-center justify-center mb-6">
					<svg class="w-6 h-6 text-warm-gray-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
						<path stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" d="M17 20h5v-2a3 3 0 00-5.356-1.857M17 20H7m10 0v-2c0-.656-.126-1.283-.356-1.857M7 20H2v-2a3 3 0 015.356-1.857M7 20v-2c0-.656.126-1.283.356-1.857m0 0a5.002 5.002 0 019.288 0M15 7a3 3 0 11-6 0 3 3 0 016 0zm6 3a2 2 0 11-4 0 2 2 0 014 0zM7 10a2 2 0 11-4 0 2 2 0 014 0z" />
					</svg>
				</div>
				<h3 class="font-display text-xl font-medium text-charcoal mb-3">
					Smart Handoffs
				</h3>
				<p class="font-body text-warm-gray-600">
					Complex requests? Special occasions? The AI knows when to connect callers to your team for that personal touch.
				</p>
			</div>
		</div>

		<!-- Social Proof -->
		<div class="mt-20 text-center">
			<p class="font-display text-warm-gray-500 italic text-lg mb-8">
				"It's like having the best host in town answer every call."
			</p>
			<div class="flex items-center justify-center gap-8 text-warm-gray-400">
				<span class="font-body text-sm">Trusted by 200+ restaurants</span>
				<span>·</span>
				<span class="font-body text-sm">50,000+ calls handled monthly</span>
			</div>
		</div>
	</div>
</section>

<!-- Pricing Section -->
<section id="pricing" class="py-24 md:py-32 bg-cream texture-overlay">
	<div class="max-w-5xl mx-auto px-6">
		<div class="text-center mb-16">
			<p class="font-display text-warm-gray-500 tracking-widest uppercase text-sm mb-4">
				Pricing
			</p>
			<h2 class="font-display text-4xl md:text-5xl font-medium text-charcoal mb-6">
				Simple, Honest Pricing
			</h2>
			<div class="decorative-line mx-auto mb-6"></div>
			<p class="font-body text-lg text-warm-gray-600 max-w-xl mx-auto">
				No hidden fees. No long contracts. Just straightforward pricing that scales with your needs.
			</p>
		</div>

		<!-- Pricing Cards -->
		<div class="grid md:grid-cols-3 gap-8">
			<!-- Starter -->
			<div class="paper-texture p-8 border border-warm-gray-200 hover:border-warm-gray-300 transition-colors">
				<h3 class="font-display text-xl font-medium text-charcoal mb-2">Starter</h3>
				<p class="font-body text-warm-gray-500 text-sm mb-6">For single locations just getting started</p>
				<div class="mb-6">
					<span class="font-display text-4xl font-medium text-charcoal">$149</span>
					<span class="font-body text-warm-gray-500">/month</span>
				</div>
				<div class="decorative-line mb-6"></div>
				<ul class="space-y-3 mb-8">
					<li class="flex items-start gap-3">
						<svg class="w-5 h-5 text-warm-gray-500 mt-0.5 flex-shrink-0" fill="none" stroke="currentColor" viewBox="0 0 24 24">
							<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7" />
						</svg>
						<span class="font-body text-warm-gray-600">Up to 500 calls/month</span>
					</li>
					<li class="flex items-start gap-3">
						<svg class="w-5 h-5 text-warm-gray-500 mt-0.5 flex-shrink-0" fill="none" stroke="currentColor" viewBox="0 0 24 24">
							<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7" />
						</svg>
						<span class="font-body text-warm-gray-600">Reservation integration</span>
					</li>
					<li class="flex items-start gap-3">
						<svg class="w-5 h-5 text-warm-gray-500 mt-0.5 flex-shrink-0" fill="none" stroke="currentColor" viewBox="0 0 24 24">
							<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7" />
						</svg>
						<span class="font-body text-warm-gray-600">Menu & allergen info</span>
					</li>
					<li class="flex items-start gap-3">
						<svg class="w-5 h-5 text-warm-gray-500 mt-0.5 flex-shrink-0" fill="none" stroke="currentColor" viewBox="0 0 24 24">
							<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7" />
						</svg>
						<span class="font-body text-warm-gray-600">Basic analytics</span>
					</li>
				</ul>
				<a href="#contact" class="btn-secondary w-full block text-center">Get Started</a>
			</div>

			<!-- Professional - Featured -->
			<div class="paper-texture p-8 border-2 border-charcoal relative">
				<div class="absolute -top-3 left-1/2 -translate-x-1/2 bg-charcoal text-cream px-4 py-1 font-display text-xs tracking-wider uppercase">
					Most Popular
				</div>
				<h3 class="font-display text-xl font-medium text-charcoal mb-2">Professional</h3>
				<p class="font-body text-warm-gray-500 text-sm mb-6">For restaurants ready to grow</p>
				<div class="mb-6">
					<span class="font-display text-4xl font-medium text-charcoal">$299</span>
					<span class="font-body text-warm-gray-500">/month</span>
				</div>
				<div class="decorative-line mb-6"></div>
				<ul class="space-y-3 mb-8">
					<li class="flex items-start gap-3">
						<svg class="w-5 h-5 text-charcoal mt-0.5 flex-shrink-0" fill="none" stroke="currentColor" viewBox="0 0 24 24">
							<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7" />
						</svg>
						<span class="font-body text-warm-gray-600">Up to 2,000 calls/month</span>
					</li>
					<li class="flex items-start gap-3">
						<svg class="w-5 h-5 text-charcoal mt-0.5 flex-shrink-0" fill="none" stroke="currentColor" viewBox="0 0 24 24">
							<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7" />
						</svg>
						<span class="font-body text-warm-gray-600">Everything in Starter</span>
					</li>
					<li class="flex items-start gap-3">
						<svg class="w-5 h-5 text-charcoal mt-0.5 flex-shrink-0" fill="none" stroke="currentColor" viewBox="0 0 24 24">
							<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7" />
						</svg>
						<span class="font-body text-warm-gray-600">Phone ordering</span>
					</li>
					<li class="flex items-start gap-3">
						<svg class="w-5 h-5 text-charcoal mt-0.5 flex-shrink-0" fill="none" stroke="currentColor" viewBox="0 0 24 24">
							<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7" />
						</svg>
						<span class="font-body text-warm-gray-600">Custom voice personality</span>
					</li>
					<li class="flex items-start gap-3">
						<svg class="w-5 h-5 text-charcoal mt-0.5 flex-shrink-0" fill="none" stroke="currentColor" viewBox="0 0 24 24">
							<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7" />
						</svg>
						<span class="font-body text-warm-gray-600">Priority support</span>
					</li>
				</ul>
				<a href="#contact" class="btn-primary w-full block text-center">Get Started</a>
			</div>

			<!-- Enterprise -->
			<div class="paper-texture p-8 border border-warm-gray-200 hover:border-warm-gray-300 transition-colors">
				<h3 class="font-display text-xl font-medium text-charcoal mb-2">Enterprise</h3>
				<p class="font-body text-warm-gray-500 text-sm mb-6">For restaurant groups & chains</p>
				<div class="mb-6">
					<span class="font-display text-4xl font-medium text-charcoal">Custom</span>
				</div>
				<div class="decorative-line mb-6"></div>
				<ul class="space-y-3 mb-8">
					<li class="flex items-start gap-3">
						<svg class="w-5 h-5 text-warm-gray-500 mt-0.5 flex-shrink-0" fill="none" stroke="currentColor" viewBox="0 0 24 24">
							<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7" />
						</svg>
						<span class="font-body text-warm-gray-600">Unlimited calls</span>
					</li>
					<li class="flex items-start gap-3">
						<svg class="w-5 h-5 text-warm-gray-500 mt-0.5 flex-shrink-0" fill="none" stroke="currentColor" viewBox="0 0 24 24">
							<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7" />
						</svg>
						<span class="font-body text-warm-gray-600">Everything in Professional</span>
					</li>
					<li class="flex items-start gap-3">
						<svg class="w-5 h-5 text-warm-gray-500 mt-0.5 flex-shrink-0" fill="none" stroke="currentColor" viewBox="0 0 24 24">
							<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7" />
						</svg>
						<span class="font-body text-warm-gray-600">Multi-location dashboard</span>
					</li>
					<li class="flex items-start gap-3">
						<svg class="w-5 h-5 text-warm-gray-500 mt-0.5 flex-shrink-0" fill="none" stroke="currentColor" viewBox="0 0 24 24">
							<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7" />
						</svg>
						<span class="font-body text-warm-gray-600">Custom integrations</span>
					</li>
					<li class="flex items-start gap-3">
						<svg class="w-5 h-5 text-warm-gray-500 mt-0.5 flex-shrink-0" fill="none" stroke="currentColor" viewBox="0 0 24 24">
							<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7" />
						</svg>
						<span class="font-body text-warm-gray-600">Dedicated account manager</span>
					</li>
				</ul>
				<a href="#contact" class="btn-secondary w-full block text-center">Contact Sales</a>
			</div>
		</div>
	</div>
</section>

<!-- Contact Section -->
<section id="contact" class="py-24 md:py-32 bg-paper halftone">
	<div class="max-w-3xl mx-auto px-6">
		<div class="text-center mb-12">
			<p class="font-display text-warm-gray-500 tracking-widest uppercase text-sm mb-4">
				Get In Touch
			</p>
			<h2 class="font-display text-4xl md:text-5xl font-medium text-charcoal mb-6">
				Let's Talk
			</h2>
			<div class="decorative-line mx-auto mb-6"></div>
			<p class="font-body text-lg text-warm-gray-600">
				Ready to transform how your restaurant handles calls? <br class="hidden md:block" />
				We'd love to hear from you.
			</p>
		</div>

		<!-- Contact Form -->
		<form class="paper-texture p-8 md:p-12 border border-warm-gray-200">
			<div class="grid md:grid-cols-2 gap-6 mb-6">
				<div>
					<label for="name" class="block font-display text-sm text-charcoal mb-2">Your Name</label>
					<input
						type="text"
						id="name"
						name="name"
						class="w-full px-4 py-3 bg-cream border border-warm-gray-200 focus:border-charcoal focus:outline-none transition-colors font-body"
						placeholder="Jane Smith"
					/>
				</div>
				<div>
					<label for="email" class="block font-display text-sm text-charcoal mb-2">Email Address</label>
					<input
						type="email"
						id="email"
						name="email"
						class="w-full px-4 py-3 bg-cream border border-warm-gray-200 focus:border-charcoal focus:outline-none transition-colors font-body"
						placeholder="jane@restaurant.com"
					/>
				</div>
			</div>
			<div class="mb-6">
				<label for="restaurant" class="block font-display text-sm text-charcoal mb-2">Restaurant Name</label>
				<input
					type="text"
					id="restaurant"
					name="restaurant"
					class="w-full px-4 py-3 bg-cream border border-warm-gray-200 focus:border-charcoal focus:outline-none transition-colors font-body"
					placeholder="The Golden Spoon"
				/>
			</div>
			<div class="mb-6">
				<label for="message" class="block font-display text-sm text-charcoal mb-2">Tell Us About Your Restaurant</label>
				<textarea
					id="message"
					name="message"
					rows="4"
					class="w-full px-4 py-3 bg-cream border border-warm-gray-200 focus:border-charcoal focus:outline-none transition-colors font-body resize-none"
					placeholder="What's your biggest phone challenge? How many calls do you get per day?"
				></textarea>
			</div>
			<button type="submit" class="btn-primary w-full">
				Start the Conversation
			</button>
		</form>

		<!-- Alternative Contact -->
		<div class="mt-12 text-center">
			<p class="font-body text-warm-gray-500 mb-4">Prefer to talk right now?</p>
			<a href="tel:+18005551234" class="font-display text-xl text-charcoal hover:text-warm-gray-600 transition-colors">
				(800) 555-1234
			</a>
		</div>
	</div>
</section>

<!-- Footer -->
<footer class="py-12 bg-charcoal text-cream">
	<div class="max-w-6xl mx-auto px-6">
		<div class="flex flex-col md:flex-row items-center justify-between gap-6">
			<div class="font-display text-2xl font-semibold">
				Table Talk
			</div>
			<div class="flex items-center gap-8">
				<a href="#product" class="text-warm-gray-400 hover:text-cream transition-colors font-body text-sm">Product</a>
				<a href="#pricing" class="text-warm-gray-400 hover:text-cream transition-colors font-body text-sm">Pricing</a>
				<a href="#contact" class="text-warm-gray-400 hover:text-cream transition-colors font-body text-sm">Contact</a>
			</div>
			<p class="font-body text-warm-gray-500 text-sm">
				&copy; 2025 Table Talk. All rights reserved.
			</p>
		</div>
	</div>
</footer>
