<script lang="ts">
	import { createEventDispatcher } from 'svelte';
	type ImageTpype = {
		src: string | HTMLImageElement;
		altText: string;
	};
	const dispatch = createEventDispatcher();
	export let imageSrc: ImageTpype;
	let imageLoaded = false;
	const onLoad = () => {
		imageLoaded = true;
		dispatch('load');
	};
</script>

<img
	class={imageLoaded ? 'loaded' : 'loading'}
	on:load={onLoad}
	src={imageSrc.src}
	alt={imageSrc.altText}
/>
<!-- placeholder iamge -->
<img
	class="placeholder {imageLoaded ?? 'loaded'}"
	src="https://via.placeholder.com/150"
	alt="loading"
/>

<style>
	img {
		display: block;
		margin: 0 auto;
		max-width: 100%;
		max-height: 100%;
	}
	img.loading {
		display: none;
	}
	.placeholder {
		display: block;
		margin: 0 auto;
		max-width: 100%;
		max-height: 100%;
	}
	.placeholder.loaded {
		display: none;
	}
</style>
