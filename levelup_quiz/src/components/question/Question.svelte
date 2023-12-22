<script>
    // IMPORTS
    import { myScore } from "../../store/store";

    // EXPORT VARIABLES
    export let questionsData;
    export let nextQuestion;

    // LOCAL VARIABLES
    let isCorrect;
    let isAnswered = false;

    const answers = questionsData["incorrect_answers"].map(answer => {
        return {
            answer: answer,
            correct: false
        }
    });

    const allAnswers = [
        ...answers,
        {
            answer: questionsData["correct_answer"],
            correct: true
        }
    ];

    const shuffle = (array) => {
        array.sort(() => Math.random() - 0.5);
    }

    shuffle(allAnswers);

    const checkAnswer = (correct) => {
        isAnswered = true;
        isCorrect = correct;

        if (correct) {
            myScore.update(score => score + 1);
        }
    }

</script>

<!-- HTML -->
<div class="question__container">
    <div class="questions__content">
        <p class="questions">{@html questionsData.question}</p>
        {#if isAnswered}
            <span class="notCorrect" class:isCorrect>
                {#if isCorrect}
                    You got it right
                {:else}
                    You goofed up
                {/if}
            </span>
        {/if}
    </div>
    <div class="answers__content">
        <div class="answers">
            {#each allAnswers as answer}
                <button
                        class="answer__button"
                        disabled={isAnswered}
                        on:click={() => checkAnswer(answer.correct)}
                >
                    {@html answer.answer}
                </button>
            {/each}
        </div>


        <div class="next__button">
            {#if isAnswered}
                <button class="next" on:click={nextQuestion}>
                    Next Question
                </button>
            {/if}
        </div>
    </div>
</div>

<!-- STYLE -->
<style>
    .question__container {

    }

    .questions__content {
        font-size: 14px;
    }

    .questions {
        margin-bottom: 10px;
    }

    .notCorrect {
        color: red;
        padding: 4px;
        border-radius: 15px;
        border: 1px solid dimgray;
        font-size: 10px;
    }

    .isCorrect {
        color: forestgreen;
        padding: 4px;
        border-radius: 15px;
        border: 1px solid dimgray;
        font-size: 10px;
    }

    .answers__content {
        width: 100%;
        background-color: red;
        margin-top: 10px;
        display: flex;
        align-items: center;
        justify-content: center;
        flex-direction: column;
    }

    .answers {}

    .answer__button {}

    .next__button {}

    .next {}
</style>