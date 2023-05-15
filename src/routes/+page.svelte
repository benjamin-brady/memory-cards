<script lang="ts">
	import FlipCard from '$lib/FlipCard.svelte';

	const emojis = ['☕️', '😀', '🔥', '🏠', '🐶', '👁️', '🐍', '💦'];
	const cardEmojies = [...emojis, ...emojis].sort((a, b) => Math.random() - 0.5);

	interface card {
		emoji: string;
		show: boolean;
		solved: boolean;
		id: number;
		clicks: number;
	}

	let cards : card[] = [];
	let clickedCards : card[] = [];
	let count = 0;

	const startNew = () => {
		clickedCards = [];
		count = 0;
		cards = cardEmojies
			.sort((a, b) => Math.random() - 0.5)
			.map((emoji, i) => {
				return { emoji, show: false, id: i, clicks: 0, solved: false };
			});
	};
	startNew();

	function showCard(card: card): any {
		if (card.show) return;
		count++;
		console.log(card);
		card.clicks++;
		card.show = true;
		clickedCards.push(card);
		if (clickedCards.length === 2) {
			// references for the timeout closure
			let card1 = clickedCards[0];
			let card2 = clickedCards[1];

			if (clickedCards[0].emoji === clickedCards[1].emoji) {
				card1.solved = true;
				card2.solved = true; 
				clickedCards = [];
			} else {
				clickedCards = [];
				console.log('unmatched cards', card1, card2);
				setTimeout(() => {
					console.log('hide cards', card1, card2);
					card1.show = false;
					card2.show = false;
					cards = cards;
				}, 1000);
			}
		}
		cards = cards;
	}

	$: solved = cards.every((card) => card.show);
</script>

<div class="grid">
	<div class="row results">
		<h1>Memory Cards</h1>
		<p>Flip the cards to find matching pairs. </p>
		{#if solved}
			<h2>🎉 You solved it! 🎉</h2>
		{/if}
		<div class="hud">
			<p><button on:click={startNew}>Restart</button></p>
			<p>Moves: <b>{count}</b> </p>
		</div>
	</div>
</div>

<div class="grid">
	{#each Array.from(Array(4).keys()) as row (row)}
		<div class="row">
			{#each Array.from(Array(4).keys()) as col (col)}
				{@const card = cards[row * 4 + col]}
				<FlipCard solved={card.solved} flipped={card.show} text={card.emoji} on:clicked={showCard(card)} />
			{/each}
		</div>
	{/each}
</div>

<style>
	.grid {
		--width: min(100vw, 80vh, 780px);
		max-width: var(--width);
		align-self: center;
		justify-self: center;
		width: 100%;
		height: 100%;
		display: flex;
		flex-direction: column;
		justify-content: flex-start;
	}

	.grid .row {
		display: grid;
		grid-template-columns: repeat(4, 1fr);
		grid-gap: 0.3rem;
		margin: 0 0 0.3rem 0;
	}

	.grid .results {
		grid-template-columns: 1fr;
	}

	.grid.playing .row.current {
		filter: drop-shadow(3px 3px 10px var(--color-bg-0));
	}

	.card.hidden {
		cursor: pointer;
	}

	.hud {
		display: flex;
		justify-content: space-between;
		font-size: 1.3em;
	}
</style>
