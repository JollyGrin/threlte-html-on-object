<script lang="ts">
	import { Canvas } from '@threlte/core';
	import Scene from '$lib/Phone/Scene.svelte';
</script>

<div class='container'>
	<Canvas>
		<Scene />
	</Canvas>
</div>

<style>
	:global(body) {
		margin: 0;
		padding: 0;
		background-color: black;
		overflow: hidden;
		width: 100vw;
		height: 100vh;
	}

	.container {
		width: 100vw;
		height: 100vh;
	}

	:global(canvas) {
		width: 100% !important;
		height: 100% !important;
	}
</style>
