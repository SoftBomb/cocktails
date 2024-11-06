<script>
    let title = 'Find your Perfect Beverage';
	let subtitle = 'Explore a world of refreshing, creative cocktail recipes. We also have beer!';
</script>

<div class="hero">
	<div class="hero-content">
		<h1>{title}</h1>
		<p>{subtitle}</p>
		<button class="cta-button">Get Started</button>
	</div>
</div>

<style>
	/* Hero section styles */
	.hero {
        position: relative;
		height: 100vh;
		background-image: url("./hero.webp");
		background-size: cover;
		background-position: center;
		display: flex;
		align-items: center;
		justify-content: center;
		color: white;
		text-align: center;
	}

    .hero-content{
        border: solid black thin;
        padding: 2%;
        background-image: url("./opacity.png");
		border-radius: 20px;
    }

	.hero h1 {
		font-size: 4rem;
		font-weight: 700;
		margin-bottom: 1rem;
		text-shadow: 2px 2px 10px rgba(0, 0, 0, 0.6);
	}

	.hero p {
		font-size: 1.5rem;
		margin-bottom: 2rem;
		text-shadow: 2px 2px 8px rgba(0, 0, 0, 0.5);
	}

	.cta-button {
		background-color: #ff6f61; /* Customize the button color */
		padding: 15px 30px;
		font-size: 1.2rem;
		color: white;
		border: none;
		border-radius: 5px;
		cursor: pointer;
		transition: background-color 0.3s ease;
	}

	.cta-button:hover {
		background-color: #e65c4d; /* Darker shade for hover effect */
	}
</style>
