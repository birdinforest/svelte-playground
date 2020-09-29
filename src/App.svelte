<script lang="ts">
	import AnswerOfAll from "./AnswerOfAll.svelte";
	import Person from "./Person.svelte";
	import LogIn from "./Login.svelte";
	import List from "./List.svelte";

	export let name: string;

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

<main>
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
