<script>
    export let questionsData;

    let { incorrect_answers } = questionsData;

</script>

<h3>{@html questionsData.question}</h3>

{#each incorrect_answers as answer}
    <button>{@html answer}</button>
{/each}