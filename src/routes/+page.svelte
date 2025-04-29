<script lang="ts">
	import { base } from '$app/paths';
	import Icon from '@iconify/svelte';
	import { writable } from 'svelte/store';
	import Arya from '../components/arya.svelte';
	import Disco from '../components/disco.svelte';
	import Doughnut from '../components/doughnut.svelte';
	import Nook from '../components/nook.svelte';
	import Pik from '../components/pik.svelte';
	import Pirate from '../components/pirate.svelte';
	import Rhythm from '../components/rhythm.svelte';
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
		pik: 'Sleepy Assistant',
		rhythm: 'Risky Rhythm',
		pirate: "Pirate's Tale"
	};

	const continuous = writable(true);
	const shuffle = writable(false);
	$: console.log($lyrics);

	const songList = writable([]);

	$: if ($shuffle) {
		$songList = $songList
			.map((value) => ({ value, sort: Math.random() }))
			.sort((a, b) => a.sort - b.sort)
			.map(({ value }) => value);
	} else {
		$songList = ['uranus', 'doughnut', 'disco', 'rhythm', 'pirate', 'arya', 'nook', 'pik'];
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
		if ($clicked == 'rhythm') $mp3 = 'Risky_Rhythm.mp3';
		if ($clicked == 'pirate') $mp3 = 'pirate.mp3';
		if (audioRef) audioRef.play();
		audioRef.addEventListener('ended', endedCallback);
	}
	let screenWidth: number = 300;
</script>

<div class="grid justify-items-center w-full" bind:clientWidth={screenWidth}>
	<div
		class="flex flex-row items-center w-full bg-slate-800 gap-2 p-2 sm:justify-between justify-end"
	>
		<div
			class:hidden={screenWidth < 640}
			class="title-font text-4xl text-center text-white justify-self-start"
		>
			Jenson's Jams
		</div>
		<div class="flex flex-row gap-2">
			<a
				class="flex flex-row py-1 px-2 border border-slate-500 bg-transparent text-white rounded-full items-center gap-2 w-min h-9 sm:h-12 max-h-9 sm:max-h-12"
				href="https://m.facebook.com/groups/257139284095779/?ref=share&mibextid=NSMWBT"
				><Icon icon="logos:facebook" class="text-xl sm:text-4xl" /> Updates
			</a>
			<!-- <a
				class="flex flex-row py-1 px-2 border border-slate-500 bg-transparent text-white rounded-full items-center gap-2 w-fit h-9 sm:h-12 max-h-9 sm:max-h-12"
				href="https://runsignup.com/Race/109004/Donate/PJSM08UTtVbvojZO"
				><img src="/sonic.png" alt="sonic" class="aspect-square inline h-full" />
				<div class="">5K Run for Charity</div>
			</a> -->
		</div>
	</div>
	<div class={`square-grid ${$lyrics ? 'p-2 pb-[110vh]' : 'p-2'} max-w-lg w-full`}>
		<div
			class:hidden={screenWidth >= 640}
			class="grid items-center text-white py-3 justify-items-center"
		>
			<div class="title-font text-5xl w-full text-center">Jenson's Jams</div>
		</div>
		<button
			on:click={() => {
				window.open('https://storage.googleapis.com/photos_and_stuff/fighter-1year.mp4', '_self');
			}}
			class="video green"
		>
			<div class:play={$clicked == 'disco'} class={`${'tada'} w-full h-full aspect-square`} />
			<span class="fun-font">Jenson's Video: <u>1 YEAR!</u></span>
		</button>
		<button
			on:click={() => {
				window.open('https://storage.googleapis.com/photos_and_stuff/wildcardwinner.MP4', '_self');
			}}
			class="video red"
		>
			<div class:play={$clicked == 'disco'} class={`${'uno'} w-full h-full aspect-square`} />
			<span class="fun-font">Jenson's Video: <u>Wild Card Winner</u></span>
		</button>
		{#each $songList as song (song)}
			<button on:click={() => ($clicked = song)} class="relative">
				{#if ['arya', 'pik', 'nook'].includes(song)}
					<div class:play={$clicked == song}>
						<img src={`${song}_clean.png`} alt={song} />
					</div>
				{:else}
					<div class:play={$clicked == song} class={`${song} w-full h-full aspect-square`} />
				{/if}
				<span class="fun-font">{titles[song]} </span>
			</button>
		{/each}
		<div class="w-1 py-20" />
	</div>
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
		{#if $clicked == 'rhythm'}
			<Rhythm />
		{/if}
		{#if $clicked == 'pirate'}
			<Pirate />
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
		@apply aspect-square h-[12vw] w-[12vw] max-w-[65px] max-h-[65px];
	}
	.square-grid button {
		transition: all 1s linear;
		@apply flex flex-row items-center border-4 border-black p-1 bg-blue-900;
	}
	.square-grid button.video.red {
		text-shadow:
			black 0 0 2px,
			black 0 0 3px;
		@apply border-4 border-yellow-700 p-1 bg-[#f60201] rounded-full overflow-hidden;
	}
	.square-grid button.video.green {
		text-shadow:
			black 0 0 2px,
			black 0 0 3px;
		color: white;
		@apply border-4 border-green-500 p-1 bg-[#156903] rounded-full overflow-hidden;
	}
	.square-grid button:has(> .play) {
		@apply bg-blue-300;
	}

	.square-grid button .play ~ span {
		@apply text-black font-semibold;
	}

	.square-grid button span {
		@apply w-full text-center text-gray-300;
	}

	.play {
		@apply border border-black;
	}
	@keyframes rock {
		0% {
			transform: rotate(-10deg);
		}
		50% {
			transform: rotate(10deg);
		}
		100% {
			transform: rotate(-10deg);
		}
	}
	.play > img {
		animation: rock 2s infinite;
	}

	.disco {
		background-image: url('/singleframe-disco.gif');
	}
	.disco:active,
	.disco:focus,
	.disco.play {
		background-image: url('/disco.gif');
	}
	.uno {
		background-image: url('/uno.jpg');
		border-radius: 100%;
	}
	.uno:active,
	.uno:focus,
	.uno.play {
		background-image: url('/uno.jpg');
	}

	.tada {
		background-image: url('/tada.png');
		border-radius: 100%;
	}
	.tada:active,
	.tada:focus,
	.tada.play {
		background-image: url('/tada.png');
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

	.rhythm {
		background-image: url('/bean.png');
	}
	.rhythm:active,
	.rhythm:focus,
	.rhythm.play {
		background-size: percentage 50% 100%;
		background-image: url('/bean.gif');
		@apply brightness-110;
	}

	.pirate {
		background-image: url('/pirate.png');
	}
	.pirate:active,
	.pirate:focus,
	.pirate.play {
		background-size: percentage 50% 100%;
		background-image: url('/pirate.gif');
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
