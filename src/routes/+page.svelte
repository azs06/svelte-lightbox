<script lang="ts">
	import LightboxGallery from '$lib/LightboxGallery/LightboxGallery.svelte';
	let images = [
		{ src: 'https://picsum.photos/id/237/200/300', altText: 'Image 1' },
		{ src: 'https://picsum.photos/id/238/200/300', altText: 'Image 2' },
		{ src: 'https://picsum.photos/id/239/200/300', altText: 'Image 3' },
		{ src: 'https://picsum.photos/id/240/200/300', altText: 'Image 4' }
	];
</script>
<LightboxGallery {images} />
