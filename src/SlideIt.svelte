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
    let glide = null;

    function initGlide(node, options) {
      const events = [
              'mount.before', 'run', 'mount.after', 'update', 'play', 'pause',
              'build.before', 'build.after', 'run.before', 'run.after',
              'run.offset', 'run.start', 'run.end', 'move', 'move.after',
              'resize', 'swipe.start', 'swipe.move', 'swipe.end', 'translate.jump',
      ];

      glide = new Glide(node, options);

      // forward glide event
      events.forEach(function (event) {
          glide.on(event, function(args) {
              // Replace event name from a.b to aB or keep source
              dispatch(event.replace(/\.\w/, (v) => v[1].toUpperCase()), args)
          });
      });

      glide.mount();

      return {
          update: glide.update,
          destroy: glide.disable
      }
    }

    const controller = {
        go: glide !== null && glide.go,
        index: glide !== null && glide.index
    };

    const bulletIn = (index) => ({
        focus: controller.go(`={index}`),
        active: controller.index === index
    })
</script>
<div use:initGlide={options} class="glide">
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
          <slot name="control" {controller}>
              <button class="glide__arrow glide__arrow--left" data-glide-dir="<">prev</button>
              <button class="glide__arrow glide__arrow--right" data-glide-dir=">">next</button>
          </slot>
      </div>
  {/if}
  {#if bullet }
      <div class="glide__bullets" data-glide-el="controls[nav]">
        {#each items as item, index}
            <slot name="bullet" props={bulletIn(index)}}>
                <button class="glide__bullet" data-glide-dir="={index}"></button>
            </slot>
        {/each}
      </div>
  {/if}
</div>
