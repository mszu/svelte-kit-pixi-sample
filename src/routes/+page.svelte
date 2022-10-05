<script>
	import "@mszu/pixi-ssr-shim";
	import { Application } from "@pixi/app";
	import { BatchRenderer, Renderer } from "@pixi/core";
	import { InteractionManager } from "@pixi/interaction";
	import { TickerPlugin } from "@pixi/ticker";
	import { Sprite } from "@pixi/sprite";
	import { Texture } from "@pixi/core";
	import { Text, TextStyle } from "@pixi/text";

	// This initialization needs to only happen once, even when the component
	// is unmounted and re-mounted
	if (!(Renderer.__plugins ?? {}).hasOwnProperty("interaction")) {
		Renderer.registerPlugin("interaction", InteractionManager);
	}
	if (!(Renderer.__plugins ?? {}).hasOwnProperty("batch")) {
		Renderer.registerPlugin("batch", BatchRenderer);
	}
	if (
		!(Application._plugins || []).some((plugin) => plugin === TickerPlugin)
	) {
		Application.registerPlugin(TickerPlugin);
	}

	import { onMount } from "svelte";

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

		const text = new Text(
			"PixiJS with SvelteKit!",
			new TextStyle({
				fontFamily: "Helvetica",
				fill: "#222",
				fontWeight: "600",
				fontSize: 48,
				stroke: "#fff",
				strokeThickness: 8,
				lineJoin: "round",
			})
		);
		text.position.set(150, 150);

		app.stage.addChild(sprite, text);

		document.getElementById("content-div").appendChild(app.view);
	});
</script>

<svelte:head>
	<title>Home</title>
</svelte:head>

<div id="content-div" class="content" />

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
