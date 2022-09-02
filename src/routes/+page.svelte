<script>
    import LightboxGallery from "$lib/LightboxGallery/LightboxGallery.svelte";
    export let count = 0;
    let images = [
        {src: "https://picsum.photos/id/237/200/300", altText: "text"},
        {src: "https://picsum.photos/id/238/200/300", altText: "text"},
        {src: "https://picsum.photos/id/239/200/300", altText: "text"},
        {src: "https://picsum.photos/id/240/200/300", altText: "text"}
    ];
</script>    
<h1>Welcome to SvelteKit</h1>
<h2>{count}</h2>
<button on:click="{() => count = count + 1}">+</button>
<p>Visit <a href="https://kit.svelte.dev">kit.svelte.dev</a> to read the documentation</p>
<LightboxGallery images={images}></LightboxGallery>
