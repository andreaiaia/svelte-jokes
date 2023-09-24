<script>
	const jokeTypes = ['Programming', 'Dark', 'Misc', 'Pun', 'Spooky', 'Christmas'];

	/**
	 * @type {string[]}
	 */
	let selectedTypes = [];
	let amount = 1;

	/**
	 * @type {string | any[]}
	 */
	let jokes = [];

	async function fetchJokes() {
		const apiUrl = `https://v2.jokeapi.dev/joke/${selectedTypes.join()}?amount=${Math.abs(amount)}`;
		const res = await fetch(apiUrl);
		const data = await res.json();
		jokes = [...data.jokes];
	}
</script>

<svelte:head>
	<title>Home</title>
	<meta name="description" content="Tell me a joke!" />
</svelte:head>

<section>
	<div>
		<h1>Welcome to this silly app!</h1>

		<h2>Choose the type of joke you want to hear, and then click the button!</h2>
	</div>

	<div class="options">
		{#each jokeTypes as type}
			<label>
				<input type="checkbox" name="jokeTypes" value={type} bind:group={selectedTypes} />
				{type}
			</label>
		{/each}

		<label>
			<input type="number" id="amount" name="amount" bind:value={amount} />
			Select how many jokes do you want.
		</label>
	</div>

	<button on:click={fetchJokes}>Fetch!</button>

	{#if jokes.length > 0}
		{#each jokes as joke, i}
			<h4>Joke #{i + 1}:</h4>
			<div>
				{#if joke.type === 'single'}
					<p>{joke.joke}</p>
				{:else}
					<p><strong>Setup:</strong> {joke.setup}</p>
					<p><strong>Delivery:</strong> {joke.delivery}</p>
				{/if}
			</div>
		{/each}
	{/if}
</section>

<style>
	section {
		width: 66ch;
		margin: 0 auto;
	}

	h1 {
		width: 100%;
	}

	h2 {
		font-size: 1.2rem;
	}

	.options {
		display: flex;
		flex-direction: column;
	}

	.options input {
		margin-right: 0.5rem;
		font-size: 2rem;
		margin-bottom: 0.5rem;
	}

	#amount {
		font-size: 1.2rem;
		width: 50px;
		margin: 1rem 0.5rem 1rem 0;
	}
</style>
