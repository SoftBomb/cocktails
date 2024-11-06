<script>
	// Importing necessary functions and components
	import { onMount } from 'svelte';
	import Dialog from '../components/cocktailPopUp/Dialog.svelte';
    import Hero from '../components/hero/hero.svelte';

	// Variables for managing dialog, selected cocktail, and cocktail data
	let dialog, selectedCocktail;
	let cocktails = [];
	let loading = true;
	let error = null;
	let selectedLetter = 'A';

	// Function to fetch cocktails based on selected letter
	async function fetchCocktails(letter) {
		// Set loading to true and clear any previous error
		loading = true;
		error = null;
		try {
			// Fetch data from the cocktail API
			const response = await fetch(
				`https://www.thecocktaildb.com/api/json/v1/1/search.php?f=${letter.toLowerCase()}`
			);
			// Parse the JSON response
			const data = await response.json();
			// Store cocktails in the array
			cocktails = data.drinks;
		} catch (err) {
			// If fetching fails, set an error message
			error = 'Failed to fetch cocktails';
		} finally {
			// Set loading to false once data is fetched
			loading = false;
		}
	}

	// Fetch data when the component is initially mounted
	onMount(() => {
		fetchCocktails(selectedLetter);
	});

	// Function to open the dialog with details of a selected cocktail
	function showCocktailSpecs(cocktail) {
		selectedCocktail = cocktail;
		dialog.showModal(); // Show the modal with cocktail details
	}

	// Function to handle letter change and fetch data for the new letter
	function handleLetterChange(event) {
		selectedLetter = event.target.value; // Update selected letter
		fetchCocktails(selectedLetter); // Fetch cocktails for new letter
	}
</script>

<!-- Main page container -->
<div class="pageContainer">
    <Hero></Hero>
	<!-- Letter selection section -->
	<div class="letterSelection">
		<span>Select a letter: </span>
		<!-- Each letter displayed as a clickable button -->
		{#each 'ABCDEFGHIJKLMNOPQRSTUVWXYZ'.split('') as letter}
			<button
				class="letterButton {selectedLetter === letter ? 'active' : ''}"
				on:click={() => handleLetterChange({ target: { value: letter } })}
			>
				{letter}
			</button>
		{/each}
	</div>

	<!-- Display loading, error, or cocktails based on current state -->
	{#if loading}
		<div class="loader">Loading...</div>
	{:else if error}
		<p class="error">{error}</p>
	{:else}
		<div>
			<h1>Cocktails starting with '{selectedLetter}'</h1>
			<!-- Container for displaying cocktails as cards -->
			<div class="cardContainer">
				{#each cocktails as cocktail}
					<div class="cocktail-card">
						<!-- Cocktail image and information -->
						<img src={cocktail.strDrinkThumb} alt={cocktail.strDrink} class="cocktail-image" />
						<div class="card-content">
							<h2>{cocktail.strDrink}</h2>
							<p>{cocktail.strInstructions}</p>
							<!-- Button to view cocktail details in dialog -->
							<button class="specs-button" on:click={() => showCocktailSpecs(cocktail)}
								>View Details</button
							>
						</div>
						<!-- Dialog component for displaying cocktail details -->
						<Dialog
							bind:dialog
							on:close={() => {
								selectedCocktail = null; // Clear selected cocktail on close
							}}
						>
							<!-- Only display dialog content if a cocktail is selected -->
							{#if selectedCocktail}
								<!-- Close button for dialog -->
								<button class="closeButton" on:click={() => dialog.close()}>X</button>

								<h2>{selectedCocktail.strDrink}</h2>
								<img
									src={selectedCocktail.strDrinkThumb}
									alt={selectedCocktail.strDrink}
									class="popUpImage"
								/>
								<p><strong>Category:</strong> {selectedCocktail.strCategory}</p>
								<p><strong>Type:</strong> {selectedCocktail.strAlcoholic}</p>
								<p><strong>Glass:</strong> {selectedCocktail.strGlass}</p>
								<p><strong>Instructions:</strong> {selectedCocktail.strInstructions}</p>
								<ul>
									{#each Array(15)
										.fill(0)
										.map((_, i) => i + 1) as i}
										{#if selectedCocktail[`strIngredient${i}`]}
											<li>
												{selectedCocktail[`strIngredient${i}`]}
												{selectedCocktail[`strMeasure${i}`]
													? ` - ${selectedCocktail[`strMeasure${i}`]}`
													: ''}
											</li>
										{/if}
									{/each}
								</ul>
							{/if}
						</Dialog>
					</div>
				{/each}
			</div>
		</div>
	{/if}
</div>

<style>
	.pageContainer {
		padding: 2rem;
		font-family: Arial, sans-serif;
		color: #333;
	}

	.letterButton {
		background-color: #f0f0f0;
		border: 1px solid #ccc;
		border-radius: 4px;
		padding: 0.5rem;
		cursor: pointer;
		transition: background-color 0.3s;
		font-size: 1rem;
	}

	.letterButton:hover {
		background-color: #e0e0e0;
	}

	.letterButton.active {
		background-color: #4caf50;
		color: #fff;
		border-color: #4caf50;
	}

	.letterSelection {
		display: flex;
		align-items: center;
		gap: 0.5rem;
		margin-bottom: 1.5rem;
		flex-wrap: wrap;
	}

	h1 {
		font-size: 1.75rem;
		color: #444;
		margin-bottom: 1rem;
		text-align: center;
	}

	.cardContainer {
		display: grid;
		grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
		gap: 1.5rem;
	}

	.cocktail-card {
		background-color: #fff;
		border: 1px solid #ddd;
		border-radius: 8px;
		overflow: hidden;
		box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
		transition: transform 0.2s ease;
	}

	.cocktail-card:hover {
		transform: scale(1.05);
	}

	.cocktail-image {
		width: 100%;
		height: 150px;
		object-fit: cover;
	}

	.card-content {
		padding: 1rem;
		text-align: center;
	}

	.cocktail-card h2 {
		font-size: 1.25rem;
		margin-bottom: 0.5rem;
		color: #333;
	}

	.cocktail-card p {
		font-size: 0.9rem;
		color: #666;
		margin-bottom: 1rem;
	}

	.specs-button {
		padding: 0.5rem 1rem;
		background-color: #4caf50;
		color: #fff;
		border: none;
		border-radius: 4px;
		cursor: pointer;
		transition: background-color 0.2s;
	}

	.specs-button:hover {
		background-color: #45a049;
	}

	.loader {
		text-align: center;
		font-size: 1.2rem;
		color: #888;
	}

	.error {
		color: red;
		text-align: center;
		margin-top: 1rem;
	}

	.popUpImage {
		width: 300px;
		margin: 1rem auto;
		border-radius: 8px;
	}

	ul {
		list-style-type: disc;
		padding-left: 1.5rem;
		margin-top: 1rem;
	}

	ul li {
		margin-bottom: 0.5rem;
	}
	.closeButton {
		position: absolute;
		top: 10px;
		right: 10px;
		background-color: #ff4d4d;
		color: white;
		border: none;
		border-radius: 50%;
		width: 30px;
		height: 30px;
		font-size: 1.2rem;
		cursor: pointer;
		display: flex;
		align-items: center;
		justify-content: center;
		transition: background-color 0.2s;
	}

	.closeButton:hover {
		background-color: #ff1a1a;
	}
</style>
