<script>
    import SlideIt from "../src/SlideIt.svelte";

    function warn({detail}) {
        alert('trigger with ' + JSON.stringify(detail))
    }
</script>
<style src="../node_modules/@glidejs/glide/dist/css/glide.theme.css"></style>
<SlideIt
        options={{
                startAt: 0,
                perView: 3
                }}
        items={[1,2,3,4,5,6,7]}>
    <div slot="item" let:item>
        <p>{item}</p>
    </div>
</SlideIt>

<SlideIt on:run={warn} items={[4,5,6]} bullet={true}>
    <div slot="item" let:item let:index>
        <p>{item}: {index}</p>
    </div>
</SlideIt>

<SlideIt on:mountBefore={warn} items={[4,5,6,7,8]} bullet={true} control={true}>
    <div slot="item" let:item>
        <p>{item}</p>
    </div>
    <div slot="control" let:glide>
        <button on:click={() => glide.go('>')}>Back</button>
        <button on:click={() => glide.go('<')}>Forward</button>
    </div>
</SlideIt>
