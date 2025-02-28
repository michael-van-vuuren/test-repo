<script>
	import { onMount } from "svelte";

	let flowers = [];
	let selectedColor = "";

	const flowerTypes = [
		"Rose",
		"Tulip",
		"Lily",
		"Orchid",
		"Daisy",
		"Sunflower",
	];
	const colors = [
		"All",
		"Indian Red",
		"Hot Pink",
		"Cornflower Blue",
		"Gold"
	];
	const flowerCount = 50;

	function getRandomFlower() {
		return {
			name: flowerTypes[Math.floor(Math.random() * flowerTypes.length)],
			color: colors[Math.floor(Math.random() * (colors.length - 1)) + 1], // Avoid "All"
			price: `$${(Math.random() * flowerCount + 5).toFixed(2)}`,
		};
	}

	onMount(() => {
		flowers = Array.from({ length: flowerCount }, getRandomFlower);
	});

	function filterFlowers(color) {
		selectedColor = color === "All" ? "" : color;
	}

	$: filteredFlowers = selectedColor
		? flowers.filter((flower) => flower.color === selectedColor)
		: flowers;
</script>

<main>
	<h1>Welcome to Our Flower Store</h1>

	<!-- Filter Buttons -->
	<div class="filter-bar">
		{#each colors as color}
			<button
				on:click={() => filterFlowers(color)}
				class:selected={selectedColor === color}
			>
				{color}
			</button>
		{/each}
	</div>

	<!-- Flower List -->
	<div class="flower-grid">
		{#each filteredFlowers as flower}
			<div class="flower-card">
				<div
					class="flower-icon"
					style="background-color: {flower.color.toLowerCase().replace(" ", "")};"
				></div>
				<h3>{flower.name}</h3>
				<p>Color: {flower.color}</p>
				<p>Price: {flower.price}</p>
			</div>
		{/each}
	</div>
</main>

<style>
	main {
		text-align: center;
		padding: 2rem;
		max-width: 800px;
		margin: auto;
	}

	.filter-bar {
		display: flex;
		justify-content: center;
		gap: 10px;
		margin-bottom: 20px;
	}

	button {
		padding: 8px 15px;
		border: none;
		background: #000000;
		color: white;
		border-radius: 5px;
		cursor: pointer;
	}

	button:hover {
		background: #434343;
	}

	button.selected {
		background: #000000;
		font-weight: bold;
	}

	.flower-grid {
		display: grid;
		grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
		gap: 20px;
		justify-content: center;
	}

	.flower-card {
		background: #f8f8f8;
		padding: 1rem;
		border-radius: 8px;
		box-shadow: 2px 2px 10px rgba(0, 0, 0, 0.1);
		text-align: center;
	}

	.flower-icon {
		width: 50px;
		height: 50px;
		border-radius: 50%;
		margin: 0 auto 10px;
	}
</style>
