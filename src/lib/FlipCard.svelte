<script lang="ts">
	import { createEventDispatcher } from 'svelte';
	const dispatch = createEventDispatcher();
	
	export let text: string;
  export let flipped = false;
  export let solved = false;
	
	function clicked() {
		dispatch('clicked', { flipped });
	}
</script>

<div class="card">
  <div class:flipped class:solved class="inner">
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
    font-size: calc(0.16 * var(--width));
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
.solved .back {
  /* border: pink solid 2px; */
}
	
.back {
  transform: rotateY(180deg);
	flex-direction: column;
}

.solved .back::after {
  animation: pulse 1000ms cubic-bezier(0.9, 0.7, 0.5, 0.9);
}

@keyframes pulse {
  0% {
    opacity: 0;
    display: block;
    z-index: 10;
  }
  50% {
    transform: scale(1.3);
    opacity: 1;
    z-index: 10;
    display: block;
  }

  100% {
    transform: scale(1);
    opacity: 0;
    content: '';
    display: none;
  }
}

.solved .back::after {
  content: '';
  position: absolute;
  width: 100%;
  height: 100%;
  top: 0;
  left: 0;
  background: rgb(147, 234, 159, 0.5);
  border-radius: 5%;
  z-index: -1;
  opacity: 0;
}
</style>