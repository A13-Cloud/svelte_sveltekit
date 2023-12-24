<script>
    // IMPORTS
    import Question from "../question/Question.svelte";
    import { fly } from "svelte/transition";
    import Modal from "../modal/Modal.svelte";
    import { myScore } from "../../store/store";

    // LOCAL VARIABLES
    let quiz = getQuiz();

    let activeQuestion = 0;
    let isModalOpen = false;

    async function getQuiz() {
        const response = await fetch("https://opentdb.com/api.php?amount=10&category=21&type=multiple");

        if (response.ok) {
            return await response.json();
        }
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
                <span class="new-quiz__bold-text">
                    start new quiz
                </span>
            </span>
        </button>
    </div>

    <div class="quiz__my-score">
        <span class="score-text">
            My Score: <span class="score-count">{$myScore}</span>
        </span>
    </div>

    <div class="quiz__question">
        <span class="question-text">Question</span>
        <span class="question-number">#{activeQuestion + 1}</span>
    </div>

    {#await quiz}
        <div class="loading-box">
            <span class="loading-text">
                Loading...
            </span>
        </div>
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
            <h2 class="modal_title">Congratulations You Won!</h2>
            <button class="start_button" on:click={resetQuiz}>Start Over</button>
        </Modal>
    {/if}
</div>


<!-- STYLE -->
<style lang="scss">
    .quiz__content {
        height: 280px;
        margin: 20px;
        border-radius: 15px;
        background-color: white;

        .new-quiz__button {
            display: flex;
            justify-content: flex-end;
            padding: 10px;
            border-radius: 15px 15px 0 0;
            background-color: #8a718d;

            .new-quiz {
                padding: 8px;
                border-radius: 15px;
                border: none;
                cursor: pointer;
                transition: color 500ms, background-color 500ms;

                &:hover {
                    background-color: #524555;
                    color: #ffffff;
                }

                .new-quiz__text {
                    text-transform: capitalize;

                    .new-quiz__bold-text {
                      font-size: 12px;
                      font-weight: bold;
                    }
                }
            }
        }

        .quiz__my-score {
            height: 30px;
            display: flex;
            justify-content: center;
            background-color: #565454;

            .score-text {
                display: flex;
                align-items: center;
                color: white;
                font-size: 14px;

                .score-count {
                    margin-left: 5px;
                    text-anchor: middle;
                    text-decoration: underline;
                }
            }
        }

        .quiz__question {
            width: 100px;
            display: flex;
            justify-content: center;
            margin: 5px 10px;
            background-color: #8a718d;
            padding: 5px;
            border-radius: 15px;
            color: #e6c4ea;
            border-bottom: 2px solid black;
            font-size: 14px;

            .question-text {
                color: white;

                .question-number {
                    margin-left: 5px;
                }
            }
        }

        .loading-box {
            width: 100%;
            display: flex;
            align-items: center;
            justify-content: center;

            .loading-text {
                height: 40px;
                width: 200px;
                border-radius: 20px;
                color: white;
                display: flex;
                align-items: center;
                justify-content: center;
                background-color: #8a718d;
                border-bottom: 4px solid black;
                visibility: hidden;

                &:before {
                    content: "Lorem ";
                    visibility: visible;
                }
            }
        }

        .question_wrapper {
            width: 450px;
            margin: 5px 10px;
            position: absolute;
            left: 50%;
            transform: translateX(-50%);
        }

        .modal_title {
          font-size: 14px;
        }

        .start_button {
          margin-top: 10px;
          font-size: 10px;
        }
    }
</style>