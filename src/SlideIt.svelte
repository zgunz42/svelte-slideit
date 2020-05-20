<!-- your code here -->
<style src="../node_modules/@glidejs/glide/dist/css/glide.core.css"></style>
<script>
    import Glide from '@glidejs/glide'
    import { createEventDispatcher } from 'svelte';

    const dispatch = createEventDispatcher();

    export let items=[];
    export let options={};
    export let bullet=false;
    export let control=false;
    function glide(node, options) {
      const events = [
              'mount.before', 'run', 'mount.after', 'update', 'play', 'pause',
              'build.before', 'build.after', 'run.before', 'run.after',
              'run.offset', 'run.start', 'run.end', 'move', 'move.after',
              'resize', 'swipe.start', 'swipe.move', 'swipe.end', 'translate.jump',
      ];
      const glide = new Glide(node, options);

      // forward glide event
      events.forEach(function (event) {
          glide.on(event, function(args) {
              dispatch(event.replace(/\.\w/, (v) => v[1].toUpperCase()), args)
          });
      });

      glide.mount();

      return {
          update(options) {
              if (glide.disabled) {
                glide.enable()
              }
          },

          destroy() {
              // glide.disable()
          }
      }
    }
</script>
<div use:glide={options} class="glide">
  <div class="glide__track" data-glide-el="track">
      <ul class="glide__slides">
        {#each items as item}
            <li class="glide__slide">
                <slot name="item" {item}>{JSON.stringify(item)}</slot>
            </li>
        {/each}
      </ul>
  </div>
  {#if control}
      <div class="glide__arrows" data-glide-el="controls">
          <button class="glide__arrow glide__arrow--left" data-glide-dir="<">prev</button>
          <button class="glide__arrow glide__arrow--right" data-glide-dir=">">next</button>
          <button class="glide__arrow glide__arrow--left" data-glide-dir="<<">first</button>
          <button class="glide__arrow glide__arrow--right" data-glide-dir=">>">last</button>
      </div>
  {/if}
  {#if bullet}
      <div class="glide__bullets" data-glide-el="controls[nav]">
        {#each items as item, index}
            <span data-glide-dir="={index}">
                <slot name="bullet">
                    <button class="glide__bullet"></button>
                </slot>
            </span>
        {/each}
      </div>
  {/if}
</div>
