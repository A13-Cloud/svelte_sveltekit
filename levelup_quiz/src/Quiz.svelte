<script>
    let quiz = getQuiz();

    async function getQuiz() {
        const response = await fetch("https://opentdb.com/api.php?amount=10&category=21&type=multiple");

        return response.json();
    }

    const updateQuestion = () => {
        quiz = getQuiz();
    }
</script>

<!-- HTML -->
<button on:click={updateQuestion}>Get New Question</button>

{#await quiz}
    Loading...
{:then data}
    <p>{data.results[0]["question"]}</p>
{/await}

<!-- STYLE -->
<style>

</style>