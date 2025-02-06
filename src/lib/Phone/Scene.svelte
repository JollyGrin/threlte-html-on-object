<script lang="ts">
	import { T } from '@threlte/core';
	import { Environment, Float, HTML, useGltf, OrbitControls } from '@threlte/extras';
	import { derived } from 'svelte/store';
	import { type Mesh, MathUtils } from 'three';
	import Geometries from './Geometries.svelte';
	import { RoundedPlaneGeometry } from './RoundedPlaneGeometry';
	

	let error = $state('');

	const gltf = useGltf<{
		nodes: {
			phone: Mesh;
		};
		materials: {};
	}>('/phone.glb');

	const phoneGeometry = derived(gltf, (gltf) => {
		if (!gltf) {
			error = 'Failed to load phone model';
			return;
		}
		if (!gltf.nodes.phone) {
			error = 'Phone model missing phone node';
			return;
		}
		return gltf.nodes.phone.geometry;
	});

	const url = 'https://threlte.xyz/';
</script>

<T.PerspectiveCamera
	position={[0, 0, 50]}
	fov={35}
	oncreate={(ref) => {
		ref.lookAt(0, 0, 0);
	}}
	makeDefault
>
	<OrbitControls enableDamping target={[0,0,0]} />
</T.PerspectiveCamera>

<!-- Debug lights -->
<T.DirectionalLight intensity={1} position={[5, 5, 5]} />
<T.DirectionalLight intensity={0.5} position={[-5, 5, -5]} />
<T.AmbientLight intensity={0.3} />

<Environment url="/shanghai_1k.hdr" />

<Float scale={0.7} floatIntensity={5}>
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

	{#if $phoneGeometry}
		<T.Mesh scale={5.65} geometry={$phoneGeometry}>
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
