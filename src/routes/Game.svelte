<script lang="ts">
	import type { Level } from './levels.ts';
	import Grid from './Grid.svelte';
	import { levels } from './levels';

	const level = levels[0];

	let size: number = level.size;
	let grid: string[] = create_grid(level);
	let found = [];

	function create_grid(level: Level): string[] {
		const copy = level.emojis.slice();
		const pairs: string[] = [];

		for (let i = 0; i <= level.size ** 2 / 2; i++) {
			const index: number = Math.floor(Math.random() * copy.length);
			const emoji: string = copy[index];

			copy.splice(index, 1);
			pairs.push(emoji);
		}


		return [];
	}
</script>

<div class="game">
	<div class="info"></div>
	<div class="grid-container">
		<Grid />
	</div>
	<div class="info"></div>
</div>

<style>
	.game {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		height: 100%;
		font-size: min(1vmin, 0.4rem);
	}

	.info {
		width: 80em;
		height: 10em;
		background: purple;
	}

	.grid-container {
		width: 80em;
		height: 80em;
		background-color: teal;
	}
</style>
