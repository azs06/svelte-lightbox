<script lang="ts">
	import { createEventDispatcher } from "svelte";
	import Icon from "./icons/Icon.svelte";
	import { ChevronRight, ChevronLeft } from "./icons/index";
    import { slide } from './slide.js';
	const dispatch = createEventDispatcher();
	type Image = {
		src: string,
		altText: string
	}
	export let images: Array<Image> = [];
	export let activeImageIndex = 0;

    const SLIDE_DURATION = 500;
    const transition_args = {
		duration: SLIDE_DURATION,
	}
	const handleClose = (e) => {
		e.preventDefault();
		e.stopPropagation();
		dispatch("close");
	};
	const getImageUrl = (imageObject: Image) => {
		return imageObject?.src;
	};
	const handleThumbnailClick = (media: Image) => {
		activeImageIndex = images.findIndex(
			(image) => image?.src == media?.src,
		);
	};
	const nextSlide = () => {
		if (activeImageIndex < images.length - 1) {
			activeImageIndex = activeImageIndex + 1;
		} else {
			prevSlide();
		}
	};
	const prevSlide = () => {
		if (activeImageIndex >= 1) {
			activeImageIndex = activeImageIndex - 1;
		} else {
			nextSlide();
		}
	};
    const gotoSlide = (direction: string) => (e:MouseEvent) => {
        e.preventDefault();
		/* if we have only one image, no need to slide */
		if(Array.isArray(images) && images.length <= 1) return;
        if(direction == 'next'){
            nextSlide();
        }else{
            prevSlide();
        }
    }
</script>

<div class="lightbox-modal">
	<div class="image-box">
		<button class="close-btn cursor-pointer" on:click="{handleClose}">
			<span class="text-white font-bold underline italic">Close</span>
		</button>
		<div class="left-nav">
			<a href="#" on:click="{gotoSlide('prev')}" class="text-white">
				<Icon
					width="{24}"
					height="{24}"
					className="color-white-900 inline"
					name="ChevronLeft"
				>
					<ChevronLeft />
				</Icon>
			</a>
		</div>
			<div 
                id="image-figure" 
                class="image-figure"
            > 
            {#each images as media, index}
                {#if activeImageIndex === index}      
                    <figure 
                        class="figure-box"                
                    >
                        <img
                            class="image"
                            in:slide={transition_args}
                            out:slide={transition_args}
                            src={media.src}
                            alt="{media.altText}"
                        />
                    </figure>
                {/if}
            {/each}
				<div class="m-0 p-0 list-navigation">
					<ul class="lightbox-modal__listNavigation list-none">
						{#each images as media, index}
							<li
								class="listNavigation-list list-none inline {index ==
								activeImageIndex
									? 'active'
									: ''}"
							>
								<span
									on:click="{handleThumbnailClick(media)}"
									class="list-box cursor-pointer"></span>
							</li>
						{/each}
					</ul>
				</div>
				<div class="m-0 p-0">
					<ul class="lightbox-modal__thubmnail">
						{#each images as media, index}
							<li
								class="thumbnail-image {index == activeImageIndex
									? 'active'
									: ''}"
							>
								<img
									on:click="{handleThumbnailClick(media)}"
									class="lightbox-modal__img cursor-pointer"
									src="{getImageUrl(media)}"
									alt="{media.altText}"
								/>
							</li>
						{/each}
					</ul>
				</div>
			</div>
		<div class="right-nav">
			<a href="#" on:click="{gotoSlide('next')}" class="text-white">
				<Icon
					width="{24}"
					height="{24}"
					className="color-gray-900 inline"
					name="ChevronLeft"
				>
					<ChevronRight />
				</Icon>
			</a>
		</div>
	</div>
</div>

<style lang="postcss">
	.lightbox-modal {
		position: fixed;
		top: 0;
		left: 0;
		width: 100%;
		height: 100vh;
		background-color: rgba(0, 0, 0, 0.7);
		display: block;
		z-index: 1000000000;
		animation: fade-in 0.4s;
	}
	.lightbox-modal .image-box {
		position: relative;
		display: flex;
		flex-direction: column;
		width: 100%;
		height: 40vh;
		margin-top: 100%;
		transform: translateY(-50%);
		background-color: black;
		justify-content: center;
		@media all and (min-width: 768px) {
			margin: 0 auto;
			width: 70vw;
			height: 80vh;
			transform: none;
			background-color: transparent;
		}
	}
	.lightbox-modal .image-figure {
		position: absolute;
		top: 50%;
		left: 50%;
		transform: translate(-50%, -50%);
		padding-top: 50px;
		@media all and (min-width: 768px) {
			max-width: 60vw;
			max-height: 60vh;
			padding-top: 0;
		}
	}
	.lightbox-modal__thubmnail {
		list-style: none;
		display: inline-flex;
		max-width: 60vw;
		overflow: hidden;
		margin-top: 10px;
	}
	.lightbox-modal__thubmnail li {
		padding: 5px 5px 5px 5px;
		display: inline;
	}
    .lightbox-modal__thubmnail li:first-child{
        padding-left: 0;
    }
    .lightbox-modal__thubmnail li:last-child{
        padding-right: 0;
    }
	.lightbox-modal__thubmnail li img.lightbox-modal__img {
		width: 45px;
		height: 45px;
		@media all and (min-width: 768px) {
			width: 60px;
			height: 60px;
		}
	}
	.lightbox-modal .image {
		max-width: 60vw;
		max-height: 60vh;
	}
	.lightbox-modal .close-btn {
		position: absolute;
		top: 20px;
		right: 20px;
		display: flex;
		justify-content: center;
		align-items: center;
		background: transparent;
		border: none;
		z-index: 10000000;
		cursor: pointer;
		@media all and (min-width: 768px) {
			right: 0;
		}
	}
	.thumbnail-image.active img {
		border: 2px solid #fff;
	}
	.list-navigation {
		position: static;
		margin-top: -30px;
		text-align: center;
	}
	.list-box {
		width: 8px;
		height: 8px;
		background-color: #ccc;
		display: inline-block;
		border-radius: 50%;
	}
	.listNavigation-list.active {
		.list-box {
			background-color: #fff;
		}
	}
	.left-nav {
		align-self: flex-start;
	}
	.right-nav {
		align-self: flex-end;
	}
    .figure-box{
        max-width: 60vw;
        max-height: 60vh;
        overflow: hidden;
    }
	@keyframes fade-in {
		from {
			opacity: 0;
		}

		to {
			opacity: 1;
		}
	}
</style>
