<script lang="ts">
    const emojis = ['☕️', '😀', '🔥', '🏠', '🐶', '👁️', '🐍', '💦']
    const cardEmojies = [...emojis, ...emojis]
		.sort((a,b) => Math.random() - 0.5);
    
    interface card {
        emoji: string,
        show: boolean,
		id: number,
		clicks: number
    }
    let cards = cardEmojies.map((emoji, i) => {
        return { emoji, show: false, id:i, clicks: 0 }
    }) as card[];
    
	let clickedCards = [] as card[];
	let count = 0;
	function showCard(card: card): any {
		if(card.show) return;
		count++;
		console.log(card);
		card.clicks++;
		card.show = true;
		clickedCards.push(card);
		if(clickedCards.length === 2) {
			if(clickedCards[0].emoji === clickedCards[1].emoji) {
				clickedCards = [];
			} else {
				// pass these references into the closure so 
				// even if the user clicks other cards before the timeout
				// the current cards are still hidden
				let card1 = clickedCards[0];
				let card2 = clickedCards[1];
				clickedCards = [];
				console.log('unmatched cards', card1, card2)
				setTimeout(() => {
					console.log('hide cards', card1, card2)
					card1.show = false;
					card2.show = false;
					cards = cards;
				}, 1000)
			}
		}
		cards = cards;
	}

	$: solved = cards.every(card => card.show);

	function flip(node, {
		delay = 0,
		duration = 200
	}) {
		return {
			delay,
			duration,
			css: (t, u) => `
				transform: rotateY(${1 - (u * 180)}deg);
				opacity: ${1 - u};
			`
		};
	}
</script>

<div class="grid">
	<div class="row results">
		<h1>Memory Cards</h1>
		<p>Flip the cards to find matching pairs.</p>
		{#if solved}
			<h2>🎉 You solved it! 🎉</h2>
		{/if}
		<p>Moves: {count}</p>
	</div>
</div>

<div class="grid">
    {#each Array.from(Array(4).keys()) as row(row)}
        <div class="row">
			{#each Array.from(Array(4).keys()) as col(col)}
				{@const card =  cards[row * 4 + col]}
					{#if card.show}
					<div class="card" on:click={() => showCard(card)} >
						{card.emoji}
					</div>
					{:else}
					<div class:hidden={!card.show} class="card" on:click={() => showCard(card)} >
						?
					</div>
					{/if}
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
		grid-gap: 0.2rem;
		margin: 0 0 0.2rem 0;
	}

	.grid .results {
		grid-template-columns: 1fr;
	}

	.grid.playing .row.current {
		filter: drop-shadow(3px 3px 10px var(--color-bg-0));
	}

	.card {
		aspect-ratio: 1;
		width: 100%;
		display: flex;
		align-items: center;
		justify-content: center;
		text-align: center;
		box-sizing: border-box;
		text-transform: lowercase;
		border: none;
		font-size: calc(0.16 * var(--width));
		border-radius: 2px;
		background: white;
		margin: 0;
		color: rgba(215, 211, 211, 0.7);
	}

	.card.hidden {
		cursor: pointer;
	}
</style>