<script lang="ts">
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
			{@render formStep(question)}
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
