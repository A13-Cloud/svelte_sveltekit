<script>
    //IMPORTS
    import Quiz from "./Quiz.svelte";
</script>

<!--  HTML  -->
<section class="level-up__app">
    <h1 class="title">
        <span class="text">
            Level Up Sport
        </span>
    </h1>
    <div class="level-up__app-content">
        <Quiz />
    </div>
</section>

<!--  STYLE  -->
<style lang="scss">
    :global(*, body) {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }

    .title {
      display: flex;
      justify-content: center;
      align-items: center;
      color: white;
      margin-bottom: 10px;

      .text {
        background-color: dimgrey;
        padding: 15px 20px;
        border-radius: 15px;
      }
    }

    .level-up__app {
        width: 100%;
        height: 100vh;
        padding: 40px 30px;
        background-color: #524555;

      .level-up__app-content {
        height: calc(100vh - 150px);
        border: 2px solid dimgrey;
        border-radius: 15px;
      }
    }
</style>