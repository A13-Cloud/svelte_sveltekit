<script>
    export let questionsData;

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
    }

</script>

<h3>{@html questionsData.question}</h3>

{#if isAnswered}
    <h4>
        {#if isCorrect}
            You got it right
        {:else}
            You goofed up
        {/if}
    </h4>
{/if}

{#each allAnswers as answer}
    <button disabled={isAnswered} on:click={() =>  checkAnswer(answer.correct)}>
        {@html answer.answer}
    </button>
{/each}