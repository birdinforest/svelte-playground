<script lang="ts">
	import AnswerOfAll from "./AnswerOfAll.svelte";
	import Person from "./Person.svelte";
	import LogIn from "./Login.svelte";
	import List from "./List.svelte";

	import {onMount} from 'svelte';
	import * as GL from '../sveltejs-gl/index'

	export let name: string;

	////////////////////////////////
	// GL
	////////////////////////////////
	export let color = '#ff3e00';
	let w = 1;
	let h = 1;
	let d = 1;

	const from_hex = hex => parseInt(hex.slice(1), 16);
	const light = {};

	onMount(() => {
		let frame;

		const loop = () => {
			frame = requestAnimationFrame(loop);

			light.x = 3 * Math.sin(Date.now() * 0.001);
			light.y = 2.5 + 2 * Math.sin(Date.now() * 0.0004);
			light.z = 3 * Math.cos(Date.now() * 0.002);
		};

		loop();

		return () => cancelAnimationFrame(frame);
	});
	////////////////////////////////

	let count = 0;
	$: doubled = count * 2;

	function handleClick() : void {
		// click on event handler
		count++;
	}

	const sandy = {
		name: 'Sandy',
		age: 37,
		job: 'Lawyer',
	}

	const iris = {
		name: 'Iris',
		age: 6,
		job: 'Student',
	}

	import Button, {Label, Icon} from '@smui/button';
	let clicked = 0;
</script>

<GL.Scene>
	<GL.Target id="center" location={[0, h/2, 0]}/>

	<GL.OrbitControls maxPolarAngle={Math.PI / 2} let:location>
		<GL.PerspectiveCamera {location} lookAt="center" near={0.01} far={1000}/>
	</GL.OrbitControls>

	<GL.AmbientLight intensity={0.3}/>
	<GL.DirectionalLight direction={[-1,-1,-1]} intensity={0.5}/>

	<!-- floor -->
	<GL.Mesh
					geometry={GL.plane()}
					location={[0,-0.01,0]}
					rotation={[-90,0,0]}
					scale={10}
					uniforms={{ color: 0xffffff }}
	/>

	<GL.Mesh
					geometry={GL.box()}
					location={[0,h/2,0]}
					rotation={[0,-20,0]}
					scale={[w,h,d]}
					uniforms={{ color: from_hex(color) }}
	/>

	<!-- spheres -->
	<GL.Mesh
					geometry={GL.sphere({ turns: 36, bands: 36 })}
					location={[-0.5, 0.4, 1.2]}
					scale={0.4}
					uniforms={{ color: 0x123456, alpha: 0.9 }}
					transparent
	/>

	<GL.Mesh
					geometry={GL.sphere({ turns: 36, bands: 36 })}
					location={[-1.4, 0.6, 0.2]}
					scale={0.6}
					uniforms={{ color: 0x336644, alpha: 0.9 }}
					transparent
	/>

	<!-- moving light -->
	<GL.Group location={[light.x,light.y,light.z]}>
		<GL.Mesh
						geometry={GL.sphere({ turns: 36, bands: 36 })}
						location={[0,0.2,0]}
						scale={0.1}
						uniforms={{ color: 0xffffff, emissive: 0xcccc99 }}
		/>

		<GL.PointLight
						location={[0,0,0]}
						color={0xffffff}
						intensity={0.6}
		/>
	</GL.Group>
</GL.Scene>

<main>
	<div class="controls">
		<label>
			<input type="color" style="height: 40px" bind:value={color}>
		</label>

		<label>
			<input type="range" bind:value={w} min={0.1} max={5} step={0.1}> width ({w})
		</label>

		<label>
			<input type="range" bind:value={h} min={0.1} max={5} step={0.1}> height ({h})
		</label>

		<label>
			<input type="range" bind:value={d} min={0.1} max={5} step={0.1}> depth ({d})
		</label>
	</div>

	<h1>Hello {name}!</h1>
	<p>Visit the <a href="https://svelte.dev/tutorial">Svelte tutorial</a> to learn how to build Svelte apps.</p>
	<p> - By Derek</p>
  <button on:click={handleClick}>
		Clicked {count} {(count === 1 || count === 0 ) ? 'time' : 'times'}
	</button>
	<p>{count} doubled is {doubled}</p>

	<!--  Component AnswerOfAll-->
	<AnswerOfAll/>
	<AnswerOfAll answer={44}/>

	<!--  Component Person-->
	<Person/>
	<Person {...sandy}/>
	<Person {...iris}/>

	<!--  Component LogIn-->
	<LogIn/>

	<!--  Component List-->
  <List/>

	<div class="container">
		<Button on:click={() => clicked++}>
			<Icon class="material-icons">thumb_up</Icon>
			<Label>Click Me</Label>
		</Button>
		<p class="mdc-typography--body1">
			{#if clicked}
				You've clicked the button {clicked} time{clicked === 1 ? '' : 's'}.
			{:else}
				<span class="grayed">You haven't clicked the button.</span>
			{/if}
		</p>
	</div>

</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	.controls {
		position: absolute;
		top: 1em;
		left: 1em;
		background-color: rgba(255,255,255,0.7);
		padding: 1em;
		border-radius: 2px;
	}

	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}

	.grayed {
		opacity: .6;
	}
</style>
