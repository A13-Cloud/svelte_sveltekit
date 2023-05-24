<script>
    import Question from "./Question.svelte";
    import {fly} from "svelte/transition";

    let quiz = getQuiz();
    let activeQuestion = 0;
    let myScore = 0;

    async function getQuiz() {
        const response = await fetch("https://opentdb.com/api.php?amount=10&category=21&type=multiple");
        return await response.json();
    }

    const nextQuestion = () => {
        activeQuestion = activeQuestion + 1;
    }

    const addToScore = () => {
        myScore = myScore + 1;
    }

    const resetQuiz = () => {
        myScore = 0;
        activeQuestion = 0;
        quiz = getQuiz();
    }

    // Reactive Statement
    $: if (myScore > 7) {
        alert("You Won!");
        resetQuiz();
    }

</script>

<!-- HTML -->
<button on:click|once={resetQuiz}>Start New Quiz</button>

<h3>My Score: {myScore}</h3>
<h4>Question #{activeQuestion + 1}</h4>

{#await quiz}
    Loading...
{:then data}

    {#each data.results as question, index}
        {#if activeQuestion === index}
            <div in:fly={{x: 100}} out:fly={{x: -200}} class="fly_wrapper">
                <Question
                        questionsData={question}
                        {nextQuestion}
                        {addToScore}
                />
            </div>
        {/if}
    {/each}

{/await}

<!-- STYLE -->
<style>
    .fly_wrapper {
        position: absolute;
    }
</style>