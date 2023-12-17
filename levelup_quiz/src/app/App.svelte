<script>
    // IMPORTS
    import Quiz from "../components/quiz/Quiz.svelte";
</script>

<!--  HTML  -->
<main class="level-up__app">
    <h1 class="title">
        <span class="text">
            Level Up Sport
        </span>
    </h1>
    <div class="level-up__app-content">
        <Quiz />
    </div>
</main>

<!--  STYLE  -->
<style lang="scss">
    :global(*, body) {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }

    .level-up__app {
      width: 100%;
      padding: 40px 30px;
      background-color: #524555;

      .title {
        display: flex;
        align-items: center;
        justify-content: center;
        color: white;
        margin-bottom: 10px;

        .text {
          font-size: 16px;
          padding: 15px 20px;
          border-radius: 15px;
          background-color: dimgrey;

        }
      }

      .level-up__app-content {
        border: 2px solid dimgrey;
        border-radius: 15px;
      }
    }
</style>