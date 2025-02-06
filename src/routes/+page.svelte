<script lang="ts">
	import { Canvas } from '@threlte/core';
	import Scene from '$lib/Phone/Scene.svelte';
</script>

<div>
	<Canvas>
		<Scene />
	</Canvas>
</div>

<style>
	:global(body) {
		background-color: black;
	}
	:global(div) {
		height: 100%;
	}
</style>
