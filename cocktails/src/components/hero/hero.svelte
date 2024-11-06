<script>
	import { onMount } from 'svelte';
	import { tweened } from 'svelte/motion';
	import { cubicOut } from 'svelte/easing';

	// For animated floating text effect
	let floatOffset = tweened(0, { duration: 3000, easing: cubicOut });

	onMount(() => {
		// Start floating animation
		floatOffset.set(20);
		floatOffset.subscribe((v) => {
			if (v === 20) floatOffset.set(-20);
			else if (v === -20) floatOffset.set(20);
		});
	});
    
	// Scroll effect for parallax background
	let scrollY = 0;
	window.addEventListener('scroll', () => {
		scrollY = window.scrollY;
		document.documentElement.style.setProperty('--scrollY', `${scrollY}px`);
	});
</script>

<div class="hero">
	<div class="parallax" style="transform: translateY(calc(var(--scrollY) * 0.3))"></div>

	<div class="content">
		<h1 class="title floating-text" style="transform: translateY({$floatOffset}px);">
			Discover the Art of Mixology
		</h1>
		<p class="subtitle">Explore our exclusive cocktail recipes and become your own mixologist!</p>
		<button class="cta" on:click={() => alert('Explore Recipes')}>Explore Recipes</button>
	</div>
</div>

<style>
	/* Full-screen hero styling */
	.hero {
		position: relative;
		width: 100%;
		height: 100vh;
		background: url('/images/hero-bg.jpg') center/cover no-repeat;
		display: flex;
		align-items: center;
		justify-content: center;
		color: #fff;
		overflow: hidden;
	}

	.content {
		text-align: center;
		max-width: 600px;
		z-index: 1;
	}

	.title {
		font-size: 4rem;
		line-height: 1.2;
		font-weight: bold;
		animation: fadeIn 1.5s ease-in-out;
	}

	.cta {
		font-size: 1.2rem;
		padding: 12px 24px;
		margin-top: 20px;
		background-color: rgba(255, 255, 255, 0.8);
		border: none;
		border-radius: 8px;
		cursor: pointer;
		transition: background-color 0.3s ease;
	}

	.cta:hover {
		background-color: #ffe900;
	}

	/* Floating effect */
	.floating-text {
		position: relative;
		transition: transform 0.3s ease-in-out;
	}

	/* Parallax effect on background */
	.parallax {
		position: absolute;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
		background: url('/images/hero-bg-overlay.png') center/cover no-repeat;
		transform: translateY(-20px);
		z-index: 0;
	}

	/* Fade-in animation */
	@keyframes fadeIn {
		0% {
			opacity: 0;
			transform: translateY(30px);
		}
		100% {
			opacity: 1;
			transform: translateY(0);
		}
	}
</style>
