<script>
    // IMPORTS
    import Question from "../question/Question.svelte";
    import {fly} from "svelte/transition";
    import Modal from "../modal/Modal.svelte";
    import {myScore} from "../../store/store";

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

    const resetQuiz = (event) => {
        isModalOpen = false;
        myScore.set(0);
        activeQuestion = 0;
        quiz = getQuiz();
        event.target.style.cursor = "no-drop";
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
                    start new quiz
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
    .quiz__content {
        height: calc(100% - 40px);
        margin: 20px;
        border-radius: 15px;
        background-color: white;
    }

    .new-quiz__button {
        display: flex;
        justify-content: flex-end;
        padding: 20px;
        border-radius: 15px 15px 0 0;
        background-color: #8a718d;
    }

    .new-quiz {
        padding: 8px;
        border-radius: 15px;
        border: none;
        cursor: pointer;
        transition: color 200ms, background-color 200ms;
    }

    .new-quiz:hover {
        background-color: #524555;
        color: #ffffff;
    }

    .new-quiz__text {
        text-transform: capitalize;
    }

    .new-quiz__bold-text {

    }

    .quiz__my-score {

    }

    .score-text {

    }

    .score-count {

    }

    .quiz__question {

    }

    .question-text {}

    .loading-text {}

    .question_wrapper {
        position: absolute;
    }





</style>