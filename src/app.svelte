<script>
  import { onMount } from "svelte";

  const MIN_Y_POSITION = -20;
  const AMOUNT_OF_ELEMENTS = 100;
  const characters = ["x", "X", "d", "D", "xd", "xD", "XD"];

  let elements = Array.from({ length: AMOUNT_OF_ELEMENTS })
    .map((_, _index) => ({
      character: characters[_index % characters.length],
      x: Math.random() * AMOUNT_OF_ELEMENTS,
      y: MIN_Y_POSITION - Math.random() * AMOUNT_OF_ELEMENTS,
      scale: 0.1 + Math.random() * 1.2,
    }))
    .sort((a, b) => a.z - b.z);

  onMount(() => {
    let frame;

    function loop() {
      frame = requestAnimationFrame(loop);

      elements = elements.map((emoji) => {
        emoji.y += 0.6 * emoji.scale;
        if (emoji.y > AMOUNT_OF_ELEMENTS + Math.abs(MIN_Y_POSITION)) {
          emoji.y = MIN_Y_POSITION;
        }

        return emoji;
      });
    }

    loop();

    return () => cancelAnimationFrame(frame);
  });
</script>

{#each elements as _element}
  <span
    style="left: {_element.x}%; top: {_element.y}%; transform: scale({_element.scale})"
    >{_element.character}</span
  >
{/each}

<style>
  :global(body) {
    overflow: hidden;
  }

  span {
    position: absolute;
    font-size: 5vw;
    user-select: none;
  }
</style>
