<script lang="ts">
	import Header from './Header.svelte';

	type FormState = {
		name: string;
		birthday: string;
		step: number;
		error: string;
		[key: string]: string | number;
	};

	let formState: FormState = $state({
		name: '',
		birthday: '',
		step: 0,
		error: ''
	});

	const questions = [
		{
			question: 'What is your name?',
			id: 'name',
			type: 'text'
		}
	];
</script>

<Header name={formState.name} />

<main>
	<p>Step: {formState.step + 1}</p>

	{#each questions as question (question.id)}
		{@render formStep(question)}
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
			<input {type} {id} bind:value={formState[id]} />
		</div>
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
