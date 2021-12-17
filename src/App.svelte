<script>
	export let name;
	var person = prompt("Type in your name");
	if (person!=null) {name = person}
	
</script>

<main>
	<h1>Hello {name}!</h1>
	<p>fuck off mate</p>
</main>

<style>
	main {
		text-align:center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	p {
		text-align: justify;
	}	
	h1 {
		color: #ff0000;
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