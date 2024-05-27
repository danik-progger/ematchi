<script lang="ts">
	import { get_twemoji_url } from "../utils";
    import { send } from "../transitions";

	export let emoji: string;
    export let selected: boolean;
    export let found: boolean;
    export let group: 'a' | 'b';
</script>

<div class="square" class:flipped={selected || found}>
    <button on:click />
    <div class="background"/>
    {#if !found}
        <img out:send={{ key: `${emoji}${group}` }} src={get_twemoji_url(emoji)} alt="emoji" />
    {/if}
</div>

<style>
	.square {
		display: flex;
		align-items: center;
		justify-content: center;
        transform-style: preserve-3d;
        transition: transform 0.4s;
	}

    .flipped {
        transform: rotateY(180deg);
    }
    .background {
        position: absolute;
        width: 100%;
        height: 100%;
        transform: rotateY(180deg);
        backface-visibility: hidden;
        background-color: white;
        border: 0.5em solid #eee;
        border-radius: 1em;
    }
    button {
        position: absolute;
        width: 100%;
        height: 100%;
        border: 0;
        font-size: inherit;
        border-radius: 1em;
    }
    img {
        height: 7em;
        width: 7em;
        pointer-events: none;
        transform: rotateY(180deg);
        backface-visibility: hidden;
    }
</style>
