<!--
Auto-generated by: https://github.com/threlte/threlte/tree/main/packages/gltf
Command: npx @threlte/gltf@3.0.0 static/models/lampRoundFloor.glb -t -s
-->

<script lang="ts">
	import type * as THREE from 'three';

	import type { Snippet } from 'svelte';
	import { T, type Props } from '@threlte/core';
	import { useCursor, useGltf } from '@threlte/extras';
	import { base } from '$app/paths';
	import { editorState } from '$lib/state.svelte';

	const { onPointerEnter, onPointerLeave } = useCursor();

	let {
		fallback,
		error,
		children,
		ref = $bindable(),
		colors,
		opacity,
		...props
	}: Props<THREE.Group> & {
		ref?: THREE.Group;
		children?: Snippet<[{ ref: THREE.Group }]>;
		fallback?: Snippet;
		error?: Snippet<[{ error: Error }]>;
		colors?: (number | string)[];
		opacity?: number;
	} = $props();

	type GLTFResult = {
		nodes: {
			Mesh_lampRoundFloor: THREE.Mesh;
			Mesh_lampRoundFloor_1: THREE.Mesh;
		};
		materials: {
			metal: THREE.MeshStandardMaterial;
			lamp: THREE.MeshStandardMaterial;
		};
	};

	let lampOn = $state(true);

	const gltf = useGltf<GLTFResult>(base + '/models/kenney-furniture-kit/lampRoundFloor.glb');
</script>

<T.Group bind:ref dispose={false} {...props}>
	{#await gltf}
		{@render fallback?.()}
	{:then gltf}
		<T.Group
			onclick={() => {
				if (editorState.isEditing) return;
				lampOn = !lampOn;
			}}
			onpointerenter={() => {
				if (editorState.isEditing) return;
				onPointerEnter();
			}}
			onpointerleave={() => {
				if (editorState.isEditing) return;
				onPointerLeave();
			}}
		>
			<T.Mesh
				castShadow
				receiveShadow
				geometry={gltf.nodes.Mesh_lampRoundFloor.geometry}
				material={gltf.materials.metal.clone()}
				material.color={colors?.[0] ?? gltf.materials.metal.color}
				material.opacity={opacity ?? gltf.materials.metal.opacity}
				material.transparent={opacity !== undefined}
				position={[-0.05, 0, 0.05]}
			/>
			<T.Mesh
				castShadow
				receiveShadow
				geometry={gltf.nodes.Mesh_lampRoundFloor_1.geometry}
				material={gltf.materials.lamp.clone()}
				material.emissive={colors?.[1] ?? '#ffffff'}
				material.opacity={opacity ?? gltf.materials.lamp.opacity}
				material.transparent={opacity !== undefined}
				position={[-0.05, 0, 0.05]}
			/>

			<T.PointLight
				position={[0, 1, 0]}
				castShadow
				color={colors?.[1] ?? '#ffffff'}
				intensity={lampOn ? 1 : 0}
			/>
		</T.Group>
	{:catch err}
		{@render error?.({ error: err })}
	{/await}

	{@render children?.({ ref })}
</T.Group>
