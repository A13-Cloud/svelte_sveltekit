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
            <span class:isCorrect>
                {#if isCorrect}
                    You got it right
                {:else}
                    You goofed up
                {/if}
            </span>
        {/if}
    </div>
    <div class="answers__content">
        {#each allAnswers as answer}
            <button disabled={isAnswered} on:click={() => checkAnswer(answer.correct)}>
                {@html answer.answer}
            </button>
        {/each}

        {#if isAnswered}
            <button on:click={nextQuestion}>Next Question</button>
        {/if}
    </div>
</div>

<!-- STYLE -->
<style>
    h5 {
        color: red;
    }

    .isCorrect {
        color: forestgreen;
    }
</style>