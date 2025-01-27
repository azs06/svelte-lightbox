### Svelte Lightbox Gallery Component

A simple lightbox gallery component for Svelte.

```svelte
<script>
  import LightboxGallery from 'simple-svelte-lightbox';
  import { onMount } from 'svelte';
  let images = [
  { src: 'https://picsum.photos/id/237/200/300', altText: 'Image 1' },
  { src: 'https://picsum.photos/id/238/200/300', altText: 'Image 2' },
  { src: 'https://picsum.photos/id/239/200/300', altText: 'Image 3' },
  { src: 'https://picsum.photos/id/240/200/300', altText: 'Image 4' }
 ];
  let showLightBox = true;  
  const onClose = () => {
    showLightBox = false
  };


</script>

<LightboxGallery showLightBox={showLightBox} images={images} onClose={onClose} />
```

### [Live Demo](https://soikat.dev/svelte-lightbox/)

### Todo
- [ ] Add more customization options
- [ ] Add more animations
- [ ] Add more tests