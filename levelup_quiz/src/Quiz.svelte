<script>
    // IMPORTS
    import Question from "./Question.svelte";
    import {fly} from "svelte/transition";
    import Modal from "./Modal.svelte";
    import {myScore} from "./store";

    // LOCAL VARIABLES
    let quiz = getQuiz();
    let activeQuestion = 0;
    let isModalOpen = false;

    async function getQuiz() {
        const response = await fetch("https://opentdb.com/api.php?amount=10&category=21&type=multiple");
        return await response.json();
    }

    const nextQuestion = () => {
        activeQuestion = activeQuestion + 1;
    }

    const resetQuiz = () => {
        isModalOpen = false;
        myScore.set(0);
        activeQuestion = 0;
        quiz = getQuiz();
    }

    // Reactive Statement
    $: if ($myScore > 0) {
        isModalOpen = true;
    }

</script>

<!-- HTML -->
<button on:click|once={resetQuiz}>Start New Quiz</button>

<h3>My Score: {$myScore}</h3>
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
                />
            </div>
        {/if}
    {/each}

{/await}

{#if isModalOpen}
    <Modal on:close={resetQuiz}>
        <h2>You Won!</h2>
        <p>Congratulations</p>
        <button on:click={resetQuiz}>Start Over</button>
    </Modal>
{/if}

<!-- STYLE -->
<style>
    .fly_wrapper {
        position: absolute;
    }
</style>