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
	const titles = {
		uranus: 'Cosmic Game',
		doughnut: 'Sweet Salvation',
		disco: 'Wild Card Winner',
		arya: 'Calico Dreams',
		nook: 'Curly Tail',
		pik: 'Sleepy Assistant'
	};

	const continuous = writable(true);
	const shuffle = writable(false);
	$: console.log($lyrics);

	const songList = writable(['uranus', 'doughnut', 'disco', 'arya', 'nook', 'pik']);

	$: if ($shuffle) {
		$songList = $songList
			.map((value) => ({ value, sort: Math.random() }))
			.sort((a, b) => a.sort - b.sort)
			.map(({ value }) => value);
	} else {
		$songList = ['uranus', 'doughnut', 'disco', 'arya', 'nook', 'pik'];
	}

	function endedCallback() {
		if (!$continuous) return;
		const index = ($songList.indexOf($clicked) + 1) % $songList.length;
		setTimeout(() => ($clicked = $songList[index]), 800);
	}

	$: if ($clicked) {
		if ($clicked == 'uranus') $mp3 = 'Cosmic_Game_done.mp3';
		if ($clicked == 'doughnut') $mp3 = 'Sweet_Salvation_V2.mp3';
		if ($clicked == 'disco') $mp3 = 'wild_card_winner.mp3';
		if ($clicked == 'arya') $mp3 = 'arya.mp3';
		if ($clicked == 'nook') $mp3 = 'nook.mp3';
		if ($clicked == 'pik') $mp3 = 'pik.mp3';
		if (audioRef) audioRef.play();
		audioRef.addEventListener('ended', endedCallback);
	}
</script>

<div class={`square-grid ${$lyrics ? 'p-2 pb-[90vh]' : 'p-2'}`}>
	{#each $songList as song (song)}
		<button on:click={() => ($clicked = song)}>
			{#if ['arya', 'pik', 'nook'].includes(song)}
				<img class:play={$clicked == song} src={`${song}_clean.png`} alt={song} />
			{:else}
				<div class:play={$clicked == song} class={`${song} w-full h-full aspect-square`} />
			{/if}
			<span class="fun-font">{titles[song]}</span>
		</button>
	{/each}
</div>
<div class="fixed bottom-0 left-0 w-full flex justify-center flex-col bg-black py-2 gap-2">
	<div class="flex justify-around w-full">
		<div>
			<input type="checkbox" hidden id="continuous-play" bind:checked={$continuous} />
			<label
				for="continuous-play"
				class={`text-sm px-2 rounded-full ${$continuous ? 'text-black bg-violet-200 ' : 'text-gray-500'}`}
				>Continuous Play</label
			>
		</div>
		<div>
			<input type="checkbox" hidden id="shuffle" bind:checked={$shuffle} />
			<label
				for="shuffle"
				class={`text-sm px-2 rounded-full ${$shuffle ? 'text-black bg-violet-200 ' : 'text-gray-500'}`}
				>Shuffle</label
			>
		</div>
		<div>
			<input type="checkbox" hidden id="lyrics" bind:checked={$lyrics} />
			<label
				for="lyrics"
				class={`text-sm px-2 rounded-full ${$lyrics ? 'text-black bg-violet-200 ' : 'text-gray-500'}`}
				>Show Lyrics</label
			>
		</div>
	</div>
	{#key $clicked}
		<div class="flex justify-around">
			<audio bind:this={audioRef} controls>
				<source src={`${base}/${$mp3}`} type="audio/mpeg" />
				Your browser does not support the audio element.
			</audio>
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
		@apply flex flex-col gap-1;
	}

	.square-grid button div,
	.square-grid button img {
		background-repeat: no-repeat;
		background-size: cover;
		background-position-x: center;
		@apply aspect-square h-[12vw] w-[12vw] max-w-[100px] max-h-[100px];
	}
	.square-grid button {
		transition: all 1s linear;
		@apply flex flex-row items-center border-4 border-black p-1 bg-blue-900;
	}
	.square-grid button:has(> .play) {
		@apply bg-blue-300;
	}

	.square-grid button .play ~ span {
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
	.uranus:active,
	.uranus:focus,
	.uranus.play {
		background-size: percentage 50% 100%;
		background-image: url('/uranus.gif');
		@apply brightness-110;
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
