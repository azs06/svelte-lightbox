<script>
    import LightboxGallery from "$lib/LightboxGallery/LightboxGallery.svelte";
    export let count = 0;
    let images = [
        {src: "https://i.picsum.photos/id/0/5616/3744.jpg", altText: "text"},
        {src: "https://i.picsum.photos/id/1/5616/3744.jpg", altText: "text"},
        {src: "https://i.picsum.photos/id/10/2500/1667.jpg", altText: "text"},
        {src: "https://i.picsum.photos/id/100/2500/1656.jpg", altText: "text"}
    ];
</script>    
<h1>Welcome to SvelteKit</h1>
<h2>{count}</h2>
<button on:click="{() => count = count + 1}">+</button>
<p>Visit <a href="https://kit.svelte.dev">kit.svelte.dev</a> to read the documentation</p>
<LightboxGallery images={images}></LightboxGallery>
