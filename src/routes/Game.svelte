<script lang="ts">
	import Countdown from './../Countdown.svelte';
	import Found from './Found.svelte';
	import type { Level } from './levels.ts';
	import Grid from './Grid.svelte';
	import { shuffle } from '../utils.js';
	import { createEventDispatcher } from 'svelte';

	const dispatcher = createEventDispatcher();

	let size: number = 0;
	let grid: string[] = [];
	let found: string[] = [];
	let remaining: number = 0;
	let duration: number = 0;
	let playing: boolean = false;

	export function start(level: Level) {
		size = 0;
		grid = [];
		found = [];
		remaining = 0;
		duration = 0;
		playing = false;

		size = level.size;
		grid = create_grid(level);
		remaining = duration = level.duration;

		resume();
	}

	export function resume() {
		playing = true;
		countdown();

		dispatcher('play');
	}

	function create_grid(level: Level): string[] {
		const copy = level.emojis.slice();
		const pairs: string[] = [];

		for (let i = 0; i < level.size ** 2 / 2; i++) {
			const index: number = Math.floor(Math.random() * copy.length);
			const emoji: string = copy[index];

			copy.splice(index, 1);
			pairs.push(emoji);
		}
		pairs.push(...pairs);

		return shuffle(pairs);
	}

	function countdown() {
		let start = Date.now();
		let remaining_at_start = remaining;

		function loop() {
			if (!playing) return;
			requestAnimationFrame(loop);
			remaining = remaining_at_start - (Date.now() - start);
			if (remaining <= 0) {
				dispatcher('loose');
				playing = false;
			}
		}

		loop();
	}
</script>

<div class="game">
	{#if playing}
		<div class="info">
			<Countdown
				{remaining}
				{duration}
				on:click={() => {
					playing = false;
					dispatcher('pause');
				}}
			/>
		</div>
	{/if}
	<div class="grid-container">
		<Grid
			{grid}
			on:found={(e) => {
				found = [...found, e.detail.emoji];
				if (found.length === (size * size) / 2) {
					dispatcher('win');
				}
			}}
			{found}
		/>
	</div>
	<div class="info">
		<Found {found} />
	</div>
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
	}

	.grid-container {
		width: 80em;
		height: 80em;
	}
</style>
