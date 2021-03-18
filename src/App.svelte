<svelte:options tag="mfs-testimonials" />

<script>
  import { onMount } from "svelte";
  import { testimonials } from "./testimonialsData.js";

  export let speed = 10;
  export const background = "#307ad5";
  export const progress = "#fdc735";

  let progressEl;
  let index = 0;
  let interval;
  let paused = false;

  const start = () => {
    progressEl.style.background = progress;
    paused = false;
    speed = 10;
    interval = setInterval(() => {
      index < testimonials.length - 1 ? index++ : (index = 0);
    }, speed * 1000);
  };

  const clear = () => {
    progressEl.style.background = "transparent";
    paused = true;
    speed = 0;
    clearInterval(interval);
  };

  onMount(() => start());
</script>

<article
  on:mouseover={clear}
  on:mouseout={start}
  style="background:{background};"
>
  <div
    bind:this={progressEl}
    class:paused
    class:play={!paused}
    style="animation-duration:{speed}s"
  />
  <p>"{testimonials[index].text}"</p>
</article>

<style>
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: Arial, Helvetica, sans-serif;
  }
  article {
    max-width: 768px;
    min-height: 260px;
    display: flex;
    flex-direction: column;
    justify-content: center;
    margin: auto;
    padding: 3em;
    background-color: #307ad5;
    color: white;
    border-radius: 25px;
  }
  p {
    line-height: 1.7;
    font-style: italic;
    text-align: justify;
  }
  div {
    height: 0.5em;
    margin-bottom: 20px;
    background: #fdc735;
    transition: 400ms;
  }
  .play {
    animation: grow linear infinite;
    transform-origin: left;
  }
  .paused {
    animation-play-state: paused;
  }
  @keyframes grow {
    0% {
      transform: scaleX(0);
    }
  }
  @media screen and (max-width: 768px) {
    article {
      padding: 2em;
    }
  }
</style>
