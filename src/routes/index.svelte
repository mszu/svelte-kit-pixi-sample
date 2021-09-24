<script context="module">
	export const prerender = true;
</script>

<script>
	import '@mszu/pixi-ssr-shim';
	import { Application } from '@pixi/app';
	import { BatchRenderer, Renderer } from '@pixi/core';
	import { InteractionManager } from '@pixi/interaction';
	import { TickerPlugin } from '@pixi/ticker';
	import { Sprite } from '@pixi/sprite';
	import { Texture } from '@pixi/core';

	// This initialization needs to only happen once, even when the component
	// is unmounted and re-mounted
	if (!(Renderer.__plugins ?? {}).hasOwnProperty('interaction')) {
		Renderer.registerPlugin('interaction', InteractionManager);
	}
	if (!(Renderer.__plugins ?? {}).hasOwnProperty('batch')) {
		Renderer.registerPlugin('batch', BatchRenderer);
	}
	if (!(Application._plugins || []).some((plugin) => plugin === TickerPlugin)) {
		Application.registerPlugin(TickerPlugin);
	}

	import { onMount } from 'svelte';
		
	let app;
		
	onMount(async () => {
		app = new Application({
			width: window.innerWidth,
			height: window.innerHeight,
			resolution: 1,
			backgroundColor: 0x10bb99,
		});

		const sprite = new Sprite(Texture.WHITE);
		sprite.tint = 0xff0000;
		sprite.width = sprite.height = 100;
		sprite.x = sprite.y = 100;

		app.stage.addChild(sprite);

		document.getElementById('content-div').appendChild(app.view);
	});
</script>

<svelte:head>
	<title>Home</title>
</svelte:head>

<div id="content-div" class="content"></div>

<style>
	.content {
		position: absolute;
		width: 100%;
		top: 0;
		left: 0;
		margin: 0;
		z-index: 1;

		box-sizing: border-box;
		display: flex;
		flex-direction: column;
		flex: auto;
	}
</style>