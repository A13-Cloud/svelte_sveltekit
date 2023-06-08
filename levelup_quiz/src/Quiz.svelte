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
    $: if ($myScore >= 3) {
        isModalOpen = true;
    }

</script>

<!-- HTML -->
<div class="quiz__content">
    <div class="new-quiz__button">
        <button class="new-quiz" on:click|once={resetQuiz}>
            <span class="new-quiz__text">
                <b class="new-quiz__bold-text">
                    Start New Quiz
                </b>
            </span>
        </button>
    </div>

    <div class="quiz__my-score">
        <span class="score-text">
            My Score: <b class="score-count">{$myScore}</b>
        </span>
    </div>

    <div class="quiz__question">
        <b class="question-text">Question</b> #{activeQuestion + 1}
    </div>

    {#await quiz}
        <span class="loading-text">
            Loading...
        </span>
    {:then data}
        {#each data.results as question, index}
            {#if activeQuestion === index}
                <div in:fly={{x: 100}} out:fly={{x: -200}} class="question_wrapper">
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
</div>


<!-- STYLE -->
<style>
    .question_wrapper {
        position: absolute;
    }
</style>