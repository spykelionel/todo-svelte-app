<script>
	export let name;
	const toggleName = _=>{
		if(name=="world"){
			name = "SERTER"
		} else {
			name = "world"
		}
	}
</script>

<main>
	<h1>Hello {name}!</h1>
	<button on:click={toggleName} type="button">Toggle Name</button>
	<p>Visit the <a href="https://svelte.dev/tutorial">Svelte tutorial</a> to learn how to build Svelte apps.</p>
</main>

<style>
	button {
		background-color: #ff3e00;
		color: white;
	}
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>