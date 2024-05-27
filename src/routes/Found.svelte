<script lang="ts">
	import { get_twemoji_url } from '../utils';
	import { receive } from '../transitions';

	export let found: string[];
</script>

<div class="found">
	{#each found as emoji}
        <div class="pair">
            <img in:receive={{ key: `${emoji}a` }} src={get_twemoji_url(emoji)} alt="emoji" />
            <img in:receive={{ key: `${emoji}b` }} src={get_twemoji_url(emoji)} alt="emoji" />
        </div>
	{/each}
</div>

<style>
	.found {
        margin: auto;
		display: flex;
		align-items: center;
		justify-content: start;
        flex-wrap: wrap;
        height: 100%;
	}
    .pair {
		height: 6em;
		width: 6em;
        margin: 2em;
		pointer-events: none;
    }
	img {
		height: 5em;
		width: 5em;
        position: absolute;
        top: 0;
        left: 0;
		pointer-events: none;
	}
</style>
