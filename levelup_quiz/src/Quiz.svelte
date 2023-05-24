<script>
    import Question from "./Question.svelte";

    let quiz = getQuiz();
    let activeQuestion = 0;
    let myScore = 0;

    async function getQuiz() {
        const response = await fetch("https://opentdb.com/api.php?amount=10&category=21&type=multiple");

        return response.json();
    }

    const nextQuestion = () => {
        activeQuestion = activeQuestion + 1;
    }

    const addToScore = () => {
        myScore = myScore + 1;
    }

    const resetQuiz = () => {
        myScore = 0;
        quiz = getQuiz();
    }

</script>

<!-- HTML -->
<button on:click={resetQuiz}>Start New Quiz</button>

<h3>My Score: {myScore}</h3>
<h4>Question #{activeQuestion + 1}</h4>

{#await quiz}
    Loading...
{:then data}

    {#each data.results as question, index}
        {#if activeQuestion === index}
            <Question
                questionsData={question}
                {nextQuestion}
                {addToScore}
            />
        {/if}
    {/each}

{/await}

<!-- STYLE -->
<style>

</style>