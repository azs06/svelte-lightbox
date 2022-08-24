<script lang="ts">
	export let width = 32;
	export let height = 32;
	export let name = "";
	export let color = "currentColor";
	export let className = "h-6 w-6";
</script>

<svg
	xmlns="http://www.w3.org/2000/svg"
	width="{width}"
	height="{height}"
	viewBox="0 0 20 20"
	aria-labelledby="{name}"
	role="presentation"
	class="{className}"
>
	<title id="{name}" lang="en">{`${name} icon`}</title>
	<g fill="{color}">
		<slot />
	</g>
</svg>

<style>
	svg {
		display: inline-block;
		vertical-align: middle;
	}
</style>
