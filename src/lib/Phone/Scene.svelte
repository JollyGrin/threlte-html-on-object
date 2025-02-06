<script lang="ts">
	import { T } from '@threlte/core';
	import { Environment, Float, HTML, useGltf, OrbitControls } from '@threlte/extras';
	import { type Mesh, MathUtils } from 'three';
	import Geometries from './Geometries.svelte';
	import { RoundedPlaneGeometry } from './RoundedPlaneGeometry';
	import { base } from '$app/paths';

	let error = $state('');

	const gltf = useGltf<{
		nodes: {
			phone: Mesh;
		};
		materials: {};
	}>(base + '/phone.glb');

	const url = 'https://dean.cafe';
</script>

<T.PerspectiveCamera position={[0, 0, 50]} fov={35} makeDefault>
	<OrbitControls enableDamping target={[0, 0, 0]} />
</T.PerspectiveCamera>

<!-- Debug lights -->
<T.DirectionalLight intensity={1} position={[5, 5, 5]} />
<T.DirectionalLight intensity={0.5} position={[-5, 5, -5]} />
<T.AmbientLight intensity={0.3} />

<Environment url={base + '/shanghai_1k.hdr'} />

<Float scale={0.7} floatIntensity={5} rotation.y={-45 * MathUtils.DEG2RAD}>
	<HTML
		position={[1.2, 0, 0]}
		rotation.y={90 * MathUtils.DEG2RAD}
		transform
		occlude="blending"
		geometry={new RoundedPlaneGeometry(10.5, 21.3, 1.6)}
	>
		<div class="phone-wrapper" style="border-radius:1rem">
			<iframe title="phone-content" src={url} width="100%" height="100%" frameborder="0"></iframe>
		</div>
	</HTML>

	{#if $gltf}
		<T.Mesh scale={5.65} geometry={$gltf.nodes.phone.geometry}>
			<T.MeshStandardMaterial color="#FF3F00" metalness={0.9} roughness={0.1} />
		</T.Mesh>
	{:else if error}
		<div style="color: red; position: absolute; top: 10px; left: 10px;">
			Error: {error}
		</div>
	{/if}
</Float>

<Geometries />

<style>
	.phone-wrapper {
		height: 848px;
		width: 420px;
		border-radius: 63px;
		overflow: hidden;
		border: 1px solid rgba(0, 0, 0, 0.1);
	}
</style>
