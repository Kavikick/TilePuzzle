<script lang="ts">
	import Board from "./Components/Board.svelte";
</script>

<main>
	<h1>Slider Puzzle!</h1>
	<Board />
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
		display: flex;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>
