<script lang="ts">
	import { createEventDispatcher } from 'svelte';
	const dispatch = createEventDispatcher();
	
	export let text: string;
    export let flipped = false;
	
	function clicked() {
		dispatch('clicked', { flipped });
	}
</script>

<div class="card">
  <div class:flipped class="inner">
		<div class="front" on:click={clicked} on:keydown={clicked}>
			?
		</div>
		<div class="back" on:click={clicked}  on:keydown={clicked}>
			{text}
		</div>
	</div>
</div>

<style>
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
    /* background: white; */
    margin: 0;
    color: rgba(215, 211, 211, 0.7);
}
.inner {
  position: relative;
  width: 100%;
  height: 100%;
  text-align: center;
  transition: transform 0.8s;
  transform-style: preserve-3d;
}
.flipped {
  transform: rotateY(180deg);
}
.front, .back {
    position: absolute;
    width: 100%;
    height: 100%;
    -webkit-backface-visibility: hidden; /* Safari */
    backface-visibility: hidden;
    display: flex;
    align-items: center;
    justify-content: center;
    gap: .5rem;
    background-color: rgb(255, 255, 255);
    box-shadow: rgba(60, 64, 67, 0.3) 0px 1px 2px 0px, rgba(60, 64, 67, 0.15) 0px 2px 6px 2px;
    border-radius: 5px;
    cursor: pointer;
}
	
.back {
  transform: rotateY(180deg);
	flex-direction: column;
}
</style>