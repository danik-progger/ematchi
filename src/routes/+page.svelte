<script lang="ts">
	import Modal from './Modal.svelte';
	import Game from './Game.svelte';
	import '../styles.css';
	import { levels } from './levels';
	import { confetti } from '@neoconfetti/svelte';

	let state: 'waiting' | 'playing' | 'paused' | 'won' | 'lost' = 'waiting';
	let game: Game;
</script>

<Game
	bind:this={game}
	on:play={() => {
		state = 'playing';
	}}
	on:loose={() => {
		state = 'lost';
	}}
	on:pause={() => {
		state = 'paused';
	}}
	on:win={() => {
		state = 'won';
	}}
/>

{#if state !== 'playing'}
	<Modal>
		<header>
			<h1>e<span>match</span>i</h1>
			<p>the emoji matching game</p>
		</header>

		{#if state === 'won' || state === 'lost'}
			<p>You {state} the game</p>
			<br />
			<br />
			<p>You can choose a level and play again</p>
		{:else if state === 'paused'}
			<p>Game paused</p>
		{:else if state === 'waiting'}
			<p>Choose a level</p>
		{/if}

		<div class="buttons">
			{#if state === 'paused'}
				<button
					on:click={() => {
						game.resume();
					}}>Resume</button
				>
				<button on:click={() => (state = 'waiting')}>Quit</button>
			{:else}
				{#each levels as level}
					<button on:click={() => game.start(level)}>{level.label}</button>
				{/each}
			{/if}
		</div>
	</Modal>
{/if}

{#if state === "won"}
    <div class="confetti" use:confetti={{
        stageWidth: innerWidth,
        stageHeight: innerHeight,
    }}></div>
{/if}

<style>
	h1 {
		font-size: 6em;
	}
	p {
		width: 100%;
		text-align: center;
		font-weight: bold;
	}
	header {
		margin-bottom: 2em;
	}
	h1 span {
		color: orange;
	}
	.buttons {
		padding: 1em 2em;
		display: flex;
		align-items: center;
		justify-content: space-between;
	}
	button {
		text-transform: capitalize;
		font-weight: bold;
		padding: 1em;
		border-radius: 0.5em;
		background-color: orange;
		border: none;
	}
    .confetti {
        position: fixed;
        width: 100%;
        height: 100%;
        top: 30%;
        left: 50%;
        pointer-events: none;
    }
</style>
