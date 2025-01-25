<script lang="ts">
	import { createEventDispatcher } from 'svelte';
	import Icon from './icons/Icon.svelte';
	import { ChevronRight, ChevronLeft } from './icons/index';
	import Image from '$lib/components/Image.svelte';

	const dispatch = createEventDispatcher();

	type Image = {
		src: string;
		altText: string;
	};

	export let images: Array<Image> = [];
	export let activeImageIndex = 0;
	export let showLightbox = true;

	const handleClose = (e: KeyboardEvent) => {
		e.preventDefault();
		e.stopPropagation();
		dispatch('close');
	};

	const handleKeydown = (e: KeyboardEvent) => {
		if (!showLightbox) return;
		if (e.key === 'Escape') {
			handleClose(e);
		}
		if (e.key === 'ArrowRight') {
			nextSlide();
		}
		if (e.key === 'ArrowLeft') {
			prevSlide();
		}
	};

	const getImageUrl = (imageObject: Image) => {
		return imageObject?.src;
	};

	const handleThumbnailClick = (media: Image) => {
		activeImageIndex = images.findIndex((image) => image?.src === media?.src);
	};

	const nextSlide = () => {
		activeImageIndex = (activeImageIndex + 1) % images.length;
	};

	const prevSlide = () => {
		activeImageIndex = (activeImageIndex - 1 + images.length) % images.length;
	};
</script>

<svelte:window on:keydown={handleKeydown} />
{#if showLightbox}
	<div class="lightbox-modal">
		<div class="image-box">
			<button class="close-btn cursor-pointer" on:click={handleClose}>
				<span class="text-white font-bold underline italic">Close</span>
			</button>

			{#if images.length > 1}
				<div class="navigation-controls">
					<div class="left-nav">
						<a href="#" on:click={prevSlide} class="nav-button">
							<Icon width={24} height={24} className="inline" name="ChevronLeft">
								<ChevronLeft />
							</Icon>
						</a>
					</div>

					<div class="right-nav">
						<a href="#" on:click={nextSlide} class="nav-button">
							<Icon width={24} height={24} className="inline" name="ChevronRight">
								<ChevronRight />
							</Icon>
						</a>
					</div>
				</div>
			{/if}

			<div id="image-figure" class="image-figure">
				<div class="slides-container">
					{#each images as media, index}
						{#if activeImageIndex === index}
							<figure class="figure-box">
								<Image
									image={media}
								/>
							</figure>
						{/if}
					{/each}
				</div>

				{#if images.length > 1}
					<div class="m-0 p-0 list-navigation">
						<ul class="lightbox-modal__listNavigation list-none">
							{#each images as media, index}
								<li
									class="listNavigation-list list-none inline {index === activeImageIndex
										? 'active'
										: ''}"
								>
									<span
										on:click={() => handleThumbnailClick(media)}
										class="list-box cursor-pointer"
									/>
								</li>
							{/each}
						</ul>
					</div>

					<div class="m-0 p-0">
						<ul class="lightbox-modal__thubmnail">
							{#each images as media, index}
								<li class="thumbnail-image {index === activeImageIndex ? 'active' : ''}">
									<img
										on:click={() => handleThumbnailClick(media)}
										class="lightbox-modal__img cursor-pointer"
										src={getImageUrl(media)}
										alt={media.altText}
									/>
								</li>
							{/each}
						</ul>
					</div>
				{/if}
			</div>
		</div>
	</div>
{/if}

<style lang="postcss">
	.lightbox-modal {
		position: fixed;
		top: 0;
		left: 0;
		width: 100%;
		height: 100vh;
		background-color: rgba(0, 0, 0, 0.7);
		display: flex;
		align-items: center;
		justify-content: center;
		z-index: 1000000000;
	}

	.image-box {
		position: relative;
		width: 100%;
		height: 100vh;
		display: flex;
		align-items: center;
		justify-content: center;
		background-color: transparent;
	}

	@media all and (min-width: 768px) {
		.image-box {
			width: 90vw;
			height: 90vh;
		}
	}

	.image-figure {
		position: relative;
		width: 100%;
		height: 100%;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		padding: 2rem;
	}

	.slides-container {
		position: relative;
		width: 100%;
		height: 100%;
		display: flex;
		align-items: center;
		justify-content: center;
	}

	.figure-box {
		position: absolute;
		top: 50%;
		left: 50%;
		transform: translate(-50%, -50%);
		margin: 0;
		padding: 0;
		width: 100%;
		height: 100%;
		display: flex;
		align-items: center;
		justify-content: center;
	}

	.image {
		max-width: 100%;
		max-height: 100%;
		object-fit: contain;
	}

	.navigation-controls {
		position: absolute;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
		display: flex;
		justify-content: space-between;
		align-items: center;
		pointer-events: none;
		padding: 0 1rem;
		z-index: 999;
	}

	.nav-button {
		background: rgba(0, 0, 0, 0.5);
		border-radius: 50%;
		padding: 0.5rem;
		display: flex;
		align-items: center;
		justify-content: center;
		pointer-events: auto;
		color: white;
		transition: background-color 0.3s ease;
	}

	.nav-button:hover {
		background: rgba(0, 0, 0, 0.8);
	}

	.close-btn {
		position: absolute;
		top: 1rem;
		right: 1rem;
		background: transparent;
		border: none;
		z-index: 10000000;
		cursor: pointer;
		color: #fff;
		padding: 0.5rem;
	}

	.lightbox-modal__thubmnail {
		list-style: none;
		display: flex;
		justify-content: center;
		gap: 0.5rem;
		margin-top: 1rem;
		padding: 0;
	}

	.lightbox-modal__thubmnail li {
		padding: 0;
	}

	.lightbox-modal__thubmnail li img.lightbox-modal__img {
		width: 45px;
		height: 45px;
		object-fit: cover;
		border-radius: 4px;
		transition: all 0.3s ease;
	}

	@media all and (min-width: 768px) {
		.lightbox-modal__thubmnail li img.lightbox-modal__img {
			width: 60px;
			height: 60px;
		}
	}

	.thumbnail-image.active img {
		border: 2px solid #fff;
		transform: scale(1.1);
	}

	.list-navigation {
		margin-top: 1rem;
	}

	.list-box {
		width: 8px;
		height: 8px;
		background-color: #ccc;
		display: inline-block;
		border-radius: 50%;
		margin: 0 0.25rem;
		cursor: pointer;
		transition: background-color 0.3s ease;
	}

	.listNavigation-list.active .list-box {
		background-color: #fff;
	}

	.lightbox-modal__listNavigation {
		list-style: none;
		margin: 0;
		padding: 0;
		display: flex;
		justify-content: center;
		gap: 0.5rem;
	}

	.listNavigation-list {
		list-style: none;
	}

	.text-white {
		color: #fff;
	}
</style>
