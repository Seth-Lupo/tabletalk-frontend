<script lang="ts">
	let menuOpen = $state(false);
</script>

<svelte:head>
	<title>Table Talk</title>
	<meta name="description" content="Never miss a call again. AI voice agent for restaurants." />
</svelte:head>

<div class="grain">

<!-- Nav -->
<nav class="fixed top-0 left-0 right-0 z-50 mix-blend-difference">
	<div class="flex justify-between items-center px-6 md:px-12 py-6">
		<a href="/" class="font-display text-lg text-cream">Table Talk</a>

		<div class="hidden md:flex gap-12 text-sm text-cream/70">
			<a href="#product" class="hover:text-cream transition-colors">Product</a>
			<a href="#pricing" class="hover:text-cream transition-colors">Pricing</a>
			<a href="#contact" class="hover:text-cream transition-colors">Contact</a>
		</div>

		<button
			class="md:hidden text-cream"
			onclick={() => menuOpen = !menuOpen}
			aria-label="Menu"
		>
			{menuOpen ? 'Close' : 'Menu'}
		</button>
	</div>
</nav>

<!-- Mobile Menu -->
{#if menuOpen}
<div class="fixed inset-0 z-40 bg-ink flex flex-col justify-center items-center gap-8">
	<a href="#product" class="font-display text-4xl text-cream" onclick={() => menuOpen = false}>Product</a>
	<a href="#pricing" class="font-display text-4xl text-cream" onclick={() => menuOpen = false}>Pricing</a>
	<a href="#contact" class="font-display text-4xl text-cream" onclick={() => menuOpen = false}>Contact</a>
</div>
{/if}


<!-- Hero -->
<section class="min-h-screen flex flex-col justify-end px-6 md:px-12 pb-24 md:pb-32">

	<div class="max-w-7xl">
		<p class="text-warm-400 text-sm tracking-wide mb-8 reveal">For restaurants that care</p>

		<h1 class="text-5xl md:text-7xl lg:text-[8rem] xl:text-[10rem] leading-[0.9] reveal delay-1">
			Never miss
		</h1>

		<div class="flex items-baseline gap-4 md:gap-8 reveal delay-2">
			<span class="text-5xl md:text-7xl lg:text-[8rem] xl:text-[10rem] font-display italic">a call</span>
			<span class="text-coral text-2xl md:text-4xl">again</span>
		</div>
	</div>

	<div class="mt-24 md:mt-32 flex flex-col md:flex-row md:items-end justify-between gap-12 reveal delay-3">
		<p class="text-warm-600 max-w-md text-lg">
			Your AI voice agent handles reservations, answers questions,
			and takes orders while you focus on your guests.
		</p>

		<a
			href="#contact"
			class="group inline-flex items-center gap-4 text-ink"
		>
			<span class="text-sm tracking-wide">Start free trial</span>
			<span class="w-12 h-px bg-ink group-hover:w-20 group-hover:bg-coral transition-all duration-500"></span>
		</a>
	</div>
</section>


<!-- Product -->
<section id="product" class="py-32 md:py-48">

	<!-- Opening statement -->
	<div class="px-6 md:px-12 mb-32 md:mb-48">
		<div class="max-w-6xl ml-auto">
			<h2 class="font-display text-4xl md:text-6xl lg:text-7xl max-w-4xl leading-[1.1]">
				A warm voice that knows your restaurant
				<span class="italic text-warm-400">inside and out</span>
			</h2>
		</div>
	</div>

	<!-- Feature: Reservations -->
	<div class="grid md:grid-cols-2 gap-8 md:gap-0 mb-32 md:mb-48 px-6 md:px-0">
		<div class="md:pl-12 md:pr-24 flex flex-col justify-center">
			<p class="text-coral text-sm tracking-wide mb-4">01</p>
			<h3 class="font-display text-3xl md:text-4xl mb-6">Reservations</h3>
			<p class="text-warm-600 text-lg">
				Connects to OpenTable, Resy, or your system. Books tables,
				confirms parties, handles special requests. No hold music.
				No missed opportunities.
			</p>
		</div>
		<div class="bg-stone h-64 md:h-auto"></div>
	</div>

	<!-- Feature: Menu Knowledge -->
	<div class="grid md:grid-cols-2 gap-8 md:gap-0 mb-32 md:mb-48 px-6 md:px-0">
		<div class="bg-stone h-64 md:h-96 order-2 md:order-1"></div>
		<div class="md:pl-24 md:pr-12 flex flex-col justify-center order-1 md:order-2">
			<p class="text-coral text-sm tracking-wide mb-4">02</p>
			<h3 class="font-display text-3xl md:text-4xl mb-6">Menu & allergens</h3>
			<p class="text-warm-600 text-lg">
				Every dish. Every ingredient. Every allergen.
				Answers dietary questions with the confidence of
				your best server.
			</p>
		</div>
	</div>

	<!-- Feature: Orders -->
	<div class="grid md:grid-cols-2 gap-8 md:gap-0 mb-32 md:mb-48 px-6 md:px-0">
		<div class="md:pl-12 md:pr-24 flex flex-col justify-center">
			<p class="text-coral text-sm tracking-wide mb-4">03</p>
			<h3 class="font-display text-3xl md:text-4xl mb-6">Phone orders</h3>
			<p class="text-warm-600 text-lg">
				Takes orders, confirms details, sends them to your kitchen.
				No third-party fees eating your margins.
				Just direct connection.
			</p>
		</div>
		<div class="bg-stone h-64 md:h-auto"></div>
	</div>

	<!-- Pull quote -->
	<div class="px-6 md:px-12 py-24 md:py-32 bg-ink text-cream">
		<blockquote class="max-w-4xl mx-auto text-center">
			<p class="font-display text-3xl md:text-5xl lg:text-6xl italic leading-[1.2] mb-12">
				"It's like having our best host answer every call"
			</p>
			<footer class="text-warm-400 text-sm tracking-wide">
				Maria Chen, The Willow Room
			</footer>
		</blockquote>
	</div>

	<!-- The difference -->
	<div class="px-6 md:px-12 py-32 md:py-48">
		<div class="max-w-3xl">
			<p class="text-coral text-sm tracking-wide mb-8">The difference</p>
			<p class="text-2xl md:text-3xl lg:text-4xl text-ink leading-relaxed">
				No robotic menus. No "press 1 for..."
				Just natural conversation that sounds like
				<em>you</em>. And when it matters most,
				a seamless handoff to your team.
			</p>
		</div>
	</div>

</section>


<!-- Pricing -->
<section id="pricing" class="px-6 md:px-12 py-32 md:py-48 bg-stone">

	<div class="max-w-6xl mx-auto">

		<div class="mb-24 md:mb-32">
			<h2 class="font-display text-4xl md:text-6xl mb-6">Pricing</h2>
			<p class="text-warm-600 max-w-md">
				No hidden fees. No long contracts.
				Cancel anytime.
			</p>
		</div>

		<div class="grid md:grid-cols-3 gap-px bg-warm-200">

			<!-- Starter -->
			<div class="bg-stone p-8 md:p-12">
				<p class="text-sm text-warm-400 mb-8">Starter</p>
				<p class="font-display text-5xl md:text-6xl mb-2">$149</p>
				<p class="text-warm-400 mb-12">/month</p>

				<ul class="space-y-4 text-warm-600">
					<li>500 calls/month</li>
					<li>Reservation integration</li>
					<li>Menu & allergen info</li>
					<li>Basic analytics</li>
				</ul>
			</div>

			<!-- Professional -->
			<div class="bg-cream p-8 md:p-12 relative">
				<span class="absolute top-0 left-0 right-0 h-1 bg-coral"></span>
				<p class="text-sm text-coral mb-8">Professional</p>
				<p class="font-display text-5xl md:text-6xl mb-2">$299</p>
				<p class="text-warm-400 mb-12">/month</p>

				<ul class="space-y-4 text-warm-600">
					<li>2,000 calls/month</li>
					<li>Everything in Starter</li>
					<li>Phone ordering</li>
					<li>Custom voice</li>
					<li>Priority support</li>
				</ul>
			</div>

			<!-- Enterprise -->
			<div class="bg-stone p-8 md:p-12">
				<p class="text-sm text-warm-400 mb-8">Enterprise</p>
				<p class="font-display text-5xl md:text-6xl mb-2">Custom</p>
				<p class="text-warm-400 mb-12">Let's talk</p>

				<ul class="space-y-4 text-warm-600">
					<li>Unlimited calls</li>
					<li>Multi-location</li>
					<li>Custom integrations</li>
					<li>Dedicated manager</li>
				</ul>
			</div>

		</div>

	</div>
</section>


<!-- Contact -->
<section id="contact" class="px-6 md:px-12 py-32 md:py-48">

	<div class="grid md:grid-cols-2 gap-16 md:gap-24 max-w-6xl mx-auto">

		<div>
			<h2 class="font-display text-4xl md:text-6xl lg:text-7xl mb-8">
				Let's<br />talk
			</h2>
			<p class="text-warm-600 text-lg mb-12">
				Ready to transform how your restaurant handles calls?
				We'd love to hear from you.
			</p>

			<div class="space-y-4 text-warm-400">
				<p>hello@tabletalk.ai</p>
				<p>(800) 555-1234</p>
			</div>
		</div>

		<form class="space-y-8">
			<div>
				<label for="name" class="block text-sm text-warm-400 mb-2">Name</label>
				<input
					type="text"
					id="name"
					name="name"
					class="w-full bg-transparent border-b border-warm-200 py-3 focus:border-coral focus:outline-none transition-colors"
				/>
			</div>

			<div>
				<label for="email" class="block text-sm text-warm-400 mb-2">Email</label>
				<input
					type="email"
					id="email"
					name="email"
					class="w-full bg-transparent border-b border-warm-200 py-3 focus:border-coral focus:outline-none transition-colors"
				/>
			</div>

			<div>
				<label for="restaurant" class="block text-sm text-warm-400 mb-2">Restaurant</label>
				<input
					type="text"
					id="restaurant"
					name="restaurant"
					class="w-full bg-transparent border-b border-warm-200 py-3 focus:border-coral focus:outline-none transition-colors"
				/>
			</div>

			<div>
				<label for="message" class="block text-sm text-warm-400 mb-2">Message</label>
				<textarea
					id="message"
					name="message"
					rows="4"
					class="w-full bg-transparent border-b border-warm-200 py-3 focus:border-coral focus:outline-none transition-colors resize-none"
				></textarea>
			</div>

			<button
				type="submit"
				class="group inline-flex items-center gap-4 mt-4"
			>
				<span class="text-sm tracking-wide">Send message</span>
				<span class="w-12 h-px bg-ink group-hover:w-20 group-hover:bg-coral transition-all duration-500"></span>
			</button>
		</form>

	</div>
</section>


<!-- Footer -->
<footer class="px-6 md:px-12 py-12 border-t border-warm-200">
	<div class="flex flex-col md:flex-row justify-between items-start md:items-center gap-6 max-w-6xl mx-auto">
		<span class="font-display text-lg">Table Talk</span>

		<div class="flex gap-8 text-sm text-warm-400">
			<a href="#product" class="hover:text-ink transition-colors">Product</a>
			<a href="#pricing" class="hover:text-ink transition-colors">Pricing</a>
			<a href="#contact" class="hover:text-ink transition-colors">Contact</a>
		</div>

		<span class="text-sm text-warm-400">&copy; 2025</span>
	</div>
</footer>

</div>
