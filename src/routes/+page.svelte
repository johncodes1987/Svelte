<script lang="ts">
	import { fly } from 'svelte/transition';
	import Header from './Header.svelte';

	type FormState = {
		answers: { [key: string]: string };
		step: number;
		error: string;
	};

	let formState: FormState = $state({
		answers: {},
		step: 0,
		error: ''
	});

	$inspect(formState.step);

	const questions = [
		{
			question: 'What is your name?',
			id: 'name',
			type: 'text'
		},
		{
			question: 'When were you born?',
			id: 'birthday',
			type: 'date'
		},
		{
			question: 'What is your favorite color?',
			id: 'favoriteColor',
			type: 'color'
		}
	];

	function nextStep(id: string) {
		if (formState.answers[id] && formState.answers[id] !== '') {
			formState.step += 1;
			formState.error = '';
		} else {
			formState.error = 'Please answer the question';
		}
	}

	// Will run onMount
	$effect(() => {
		console.log('on mounted');
		return () => {
			// when unmounted or destroyed
			// before effect Re-runs
			console.log('on unmounted');
		};
	});

	$effect(() => {
		// This will re-run every time formState changes
		console.log('formState', formState.step);
		// DON'T create state based off other state, in effects to avoid infinite loops
		// use $derived()
		return () => {
			// before effect Re-runs
			console.log('before formState re-runs', formState.step);
		};
	});
</script>

<Header name={formState.answers.name} />

<main>
	{#if formState.step >= questions.length}
		<p>Thank You!</p>
	{:else}
		<p>Step: {formState.step + 1}</p>
	{/if}

	{#each questions as question, index (question.id)}
		{#if formState.step === index}
			<div 
				in:fly={{ x: 200, duration: 200, opacity: 0, delay: 200 }} 
				out:fly={{ x: -200, duration: 200 }}>
				{@render formStep(question)}
			</div>
		{/if}
	{/each}

	<!-- {#each questions as { id, question, type } (id)}
		{@render formStep({ question, id, type })}
	{/each} -->

	{#if formState.error}
		<p class="error">{formState.error}</p>
	{/if}
</main>

{#snippet formStep({ question, id, type }: { question: string; id: string; type: string })}
	<article>
		<div>
			<label for={id}>{question}</label>
			<input {type} {id} bind:value={formState.answers[id]} />
		</div>
		<button onclick={() => nextStep(id)}>Next</button>
	</article>
{/snippet}

<style>
	/* div {
        background: blue;
    } */
	/* :global(div) {
        background: blue;
    } */
	/* Above would make every div in your webpage blue */
	.error {
		color: red;
		font-weight: bold;
	}
</style>
