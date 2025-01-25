<script lang="ts">
	import { createEventDispatcher, onMount } from 'svelte';
	const dispatch = createEventDispatcher();

	interface ImageType {
		src: string;
		altText: string;
		placeholder?: string;
		width?: number;
		height?: number;
	}

	export let image: ImageType;
	export let pictureClass = '';
	export let imgClass = '';
	export let lazy = true;
	export let preload = false;

	let imageLoaded = false;
	let imageInstance: HTMLImageElement;
	let observer: IntersectionObserver;
	let shouldLoad = preload || !lazy;

	const onLoad = () => {
		imageLoaded = true;
		dispatch('load');
	};

	onMount(() => {
		if (!lazy || preload) return;

		observer = new IntersectionObserver(
			(entries) => {
				if (entries[0].isIntersecting) {
					shouldLoad = true;
					observer?.disconnect();
				}
			},
			{
				rootMargin: '50px'
			}
		);

		if (imageInstance) {
			observer.observe(imageInstance);
		}

		return () => {
			observer?.disconnect();
		};
	});
</script>

<div class="image-wrapper {pictureClass}">
	<img
		class="image {imgClass}"
		class:loaded={imageLoaded}
		style="opacity: {imageLoaded || preload ? 1 : 0}"
		srcset={image.placeholder ? `${image.placeholder} 1x, ${image.src} 2x` : image.src}
		alt={image.altText}
		width={image.width}
		height={image.height}
		bind:this={imageInstance}
		on:load={onLoad}
		loading={lazy ? 'lazy' : 'eager'}
	/>
</div>

<style lang="postcss">
	.image-wrapper {
		width: 100%;
		height: 100%;
		position: relative;
		display: flex;
		align-items: center;
		justify-content: center;
	}

	.image {
		max-width: 100%;
		max-height: 100%;
		width: auto;
		height: auto;
		object-fit: contain;
		transition: opacity 0.3s ease;
	}

	.image:not(.loaded) {
		opacity: 0;
	}

	.placeholder {
		position: absolute;
		top: 50%;
		left: 50%;
		transform: translate(-50%, -50%);
		max-width: 100%;
		max-height: 100%;
		width: auto;
		height: auto;
		object-fit: contain;
		filter: blur(10px);
		transform: scale(1.1) translate(-45%, -45%);
		transition: opacity 0.3s ease;
	}

	.loaded + .placeholder {
		opacity: 0;
	}
</style>
