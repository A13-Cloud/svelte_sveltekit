<!-- JS -->
<script>
    // IMPORTS
    import {myScore} from "../../store/store";

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
<h3>{@html questionsData.question}</h3>

{#if isAnswered}
    <h5 class:isCorrect>
        {#if isCorrect}
            You got it right
        {:else}
            You goofed up
        {/if}
    </h5>
{/if}

{#each allAnswers as answer}
    <button disabled={isAnswered} on:click={() => checkAnswer(answer.correct)}>
        {@html answer.answer}
    </button>
{/each}

{#if isAnswered}
    <button on:click={nextQuestion}>Next Question</button>
{/if}

<!-- STYLE -->
<style>
    h5 {
        color: red;
    }

    .isCorrect {
        color: forestgreen;
    }
</style>