<script lang="ts">
	import { base } from '$app/paths';
	import { writable } from 'svelte/store';
	import Disco from '../components/disco.svelte';
	import Doughnut from '../components/doughnut.svelte';
	import Uranus from '../components/uranus.svelte';
	const clicked = writable('');
	const mp3 = writable('');
	let audioRef: HTMLAudioElement;

	$: if ($clicked) {
		if ($clicked == 'uranus') $mp3 = 'Cosmic_Game_done.mp3';
		if ($clicked == 'doughnut') $mp3 = 'Sweet_Salvation_V2.mp3';
		if ($clicked == 'disco') $mp3 = 'wild_card_winner.mp3';
		if (audioRef) audioRef.play();
	}
</script>

<div class="square-grid">
	<button
		class:doughnut_play={$clicked == 'doughnut'}
		class={`doughnut w-full h-full aspect-square`}
		on:click={() => ($clicked = 'doughnut')}><span>Sweet Sensation</span></button
	>
	<button
		class:uranus_play={$clicked == 'uranus'}
		class={`uranus w-full h-full aspect-square`}
		on:click={() => ($clicked = 'uranus')}><span>Cosmic Game</span></button
	>
	<button
		class:disco_play={$clicked == 'disco'}
		class={`disco w-full h-full aspect-square`}
		on:click={() => ($clicked = 'disco')}><span>Wild Card Winner</span></button
	>
</div>
<div class="grid place-items-center text-white pt-10">
	{#if $clicked == 'disco'}
		<Disco />
	{/if}
	{#if $clicked == 'doughnut'}
		<Doughnut />
	{/if}
	{#if $clicked == 'uranus'}
		<Uranus />
	{/if}
</div>
<div class="fixed bottom-0 left-0 w-full flex justify-center bg-black py-2">
	{#key $clicked}
		<audio bind:this={audioRef} controls>
			<source src={`${base}/${$mp3}`} type="audio/mpeg" />
			Your browser does not support the audio element.
		</audio>
	{/key}
</div>

<style lang="postcss">
	.square-grid {
		grid-template-columns: repeat(auto-fit, minmax(100px, 1fr));
		@apply grid gap-4 h-fit max-w-3xl m-auto p-3 justify-center;
	}

	button {
		background-repeat: no-repeat;
		@apply relative text-gray-600 border-8 border-slate-700 rounded-md aspect-square;
	}
	button span {
		text-shadow:
			0 0 10px black,
			0 0 8px black;
		transition: filter 1s linear;
		@apply absolute left-1/2 bottom-1 -translate-x-1/2 blur-[2px];
	}

	.disco {
		background-size: contain;
		background-image: url('/singleframe-disco.gif');
	}
	.disco:active,
	.disco:focus,
	.disco_play {
		background-size: contain;
		background-image: url('/disco.gif');
		@apply shadow-xl text-white border-green-300;
	}

	.disco_play span,
	.doughnut_play span,
	.uranus_play span {
		@apply blur-0;
	}

	.doughnut {
		background-size: contain;
		background-image: url('/singleframe-doughnut.gif');
	}
	.doughnut:active,
	.doughnut:focus,
	.doughnut_play {
		background-size: contain;
		background-image: url('/doughnut.gif');
		@apply shadow-sm  text-white border-green-300;
	}

	.uranus {
		background-size: contain;
		background-image: url('/singleframe-uranus.gif');
	}
	.uranus:active,
	.uranus:focus,
	.uranus_play {
		background-size: contain;
		background-image: url('/uranus.gif');
		@apply shadow-sm text-white border-green-300;
	}
</style>
