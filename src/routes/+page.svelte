<script lang="ts">
	import { base } from '$app/paths';
	import { writable } from 'svelte/store';
	import Arya from '../components/arya.svelte';
	import Disco from '../components/disco.svelte';
	import Doughnut from '../components/doughnut.svelte';
	import Nook from '../components/nook.svelte';
	import Pik from '../components/pik.svelte';
	import Uranus from '../components/uranus.svelte';
	const clicked = writable('');
	const lyrics = writable(false);
	const mp3 = writable('');
	let audioRef: HTMLAudioElement;

	$: if ($clicked) {
		if ($clicked == 'uranus') $mp3 = 'Cosmic_Game_done.mp3';
		if ($clicked == 'doughnut') $mp3 = 'Sweet_Salvation_V2.mp3';
		if ($clicked == 'disco') $mp3 = 'wild_card_winner.mp3';
		if ($clicked == 'arya') $mp3 = 'arya.mp3';
		if ($clicked == 'nook') $mp3 = 'nook.mp3';
		if ($clicked == 'pik') $mp3 = 'pik.mp3';
		if (audioRef) audioRef.play();
	}
</script>

<div class={`square-grid ${$lyrics ? 'p-2 pb-[80vh]' : 'p-2'}`}>
	<button on:click={() => ($clicked = 'doughnut')}>
		<div class:play={$clicked == 'doughnut'} class={`doughnut w-full h-full aspect-square`} />
		<span>Sweet Sensation</span>
	</button>

	<button on:click={() => ($clicked = 'uranus')}>
		<div class:play={$clicked == 'uranus'} class={`uranus w-full h-full aspect-square`} />
		<span>Cosmic Game</span>
	</button>

	<button on:click={() => ($clicked = 'disco')}>
		<div class:play={$clicked == 'disco'} class={`disco w-full h-full aspect-square`} />
		<span>Wild Card Winner</span>
	</button>

	<button on:click={() => ($clicked = 'arya')}>
		<div class:play={$clicked == 'arya'} class={`arya w-full h-full aspect-square`} />
		<span>Calico Dreams</span>
	</button>

	<button on:click={() => ($clicked = 'nook')}>
		<div class:play={$clicked == 'nook'} class={`nook w-full h-full aspect-square`} />
		<span>Curly Tail</span>
	</button>

	<button on:click={() => ($clicked = 'pik')}>
		<div class:play={$clicked == 'pik'} class={`pik w-full h-full aspect-square`} />
		<span>Sleepy Assistant</span>
	</button>
</div>
<div class="fixed bottom-0 left-0 w-full flex justify-center flex-col bg-black py-2">
	{#key $clicked}
		<div class="flex justify-around">
			<audio bind:this={audioRef} controls>
				<source src={`${base}/${$mp3}`} type="audio/mpeg" />
				Your browser does not support the audio element.
			</audio>
			<button
				class="text-white rounded-full border border-gray-500 px-2"
				on:click={() => ($lyrics = !$lyrics)}>Lyrics</button
			>
		</div>
	{/key}
	<details open={$lyrics} class="text-white">
		<summary></summary>
		<br />
		{#if $clicked == 'disco'}
			<Disco />
		{/if}
		{#if $clicked == 'doughnut'}
			<Doughnut />
		{/if}
		{#if $clicked == 'uranus'}
			<Uranus />
		{/if}
		{#if $clicked == 'pik'}
			<Pik />
		{/if}
		{#if $clicked == 'arya'}
			<Arya />
		{/if}
		{#if $clicked == 'nook'}
			<Nook />
		{/if}
	</details>
</div>

<style lang="postcss">
	.square-grid {
		/* grid-template-columns: repeat(auto-fit, minmax(100px, 1fr));
		@apply grid gap-4 h-fit max-w-3xl m-auto p-3 justify-center; */
		@apply flex flex-col gap-1;
	}

	.square-grid button div {
		background-repeat: no-repeat;
		background-size: cover;
		background-position-x: center;
		@apply aspect-square h-[12vw] w-[12vw] max-w-[100px] max-h-[100px];
	}
	.square-grid button {
		@apply flex flex-row items-center border-4 border-black p-1 bg-blue-900;
	}
	.square-grid button:has(> .play) {
		@apply bg-blue-300;
	}

	.square-grid button div.play ~ span {
		@apply text-black font-semibold;
	}

	.square-grid button span {
		@apply w-full text-center text-gray-400;
	}

	.play {
		@apply border border-black;
	}

	.disco {
		background-image: url('/singleframe-disco.gif');
	}
	.disco:active,
	.disco:focus,
	.disco.play {
		background-image: url('/disco.gif');
	}

	.doughnut {
		background-image: url('/singleframe-doughnut.gif');
	}
	.doughnut:active,
	.doughnut:focus,
	.doughnut.play {
		background-size: percentage 50% 100%;
		background-image: url('/doughnut.gif');
	}

	.uranus {
		background-image: url('/singleframe-uranus.gif');
	}
	.arya {
		background-image: url('/arya_clean.png');
	}
	.nook {
		background-image: url('/nook_clean.png');
	}
	.pik {
		background-image: url('/pik_clean.png');
	}
	details > summary {
		list-style: none;
	}
	details > summary::-webkit-details-marker {
		display: none;
	}
</style>
