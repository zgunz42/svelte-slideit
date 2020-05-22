## Svelte SlideIt

![npm-publish](https://github.com/zgunz42/svelte-slideit/workflows/npm-publish/badge.svg) [![svelte-v2](https://img.shields.io/badge/svelte-v2-orange.svg)](https://v2.svelte.dev) [![svelte-v3](https://img.shields.io/badge/svelte-v3-blueviolet.svg)](https://svelte.dev)

### Hello SlideIt

SlideIt is a slider and carousel build on top of [Glidejs](https://glidejs.com/). I build this because other
svelte component like *[svelte-carousel](https://github.com/beyonk-adventures/svelte-carousel/blob/master/README.md)* 
did not support responsive because can't handle it [Siema](https://github.com/pawelgrzybek/siema). Other reason i need
carousel API like owlcarousel but use vanilla js for less depedency.

Core feature:

- [x] Responsive config options
- [x] Port all glidejs event to svelte custom event
- [x] Many slot that can you fill with "any html tag"
- [x] Glidejs css imported in style tag thank to **svelte-preprocess**

## Event Name

All event name from Glidejs has been rename `a.b` into `aB`, so if want to listen
into some event like this.

```sveltehtml
<SlideIt on:montBefore={handler} />
``` 
It will listen into `mount.before` of Glidejs event name

## Render Carousel Item

```sveltehtml
<SlideIt items={[4,5,6]}>
    <div slot="item" let:item>
        <p>{item}</p>
    </div>
</SlideIt>
```

## Render Control and Bullet

_TODO_

## Same Config as Glide js

See glidejs documentation https://glidejs.com/docs/options/
