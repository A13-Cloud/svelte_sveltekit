<script>
    // IMPORTS
    import { fade, fly} from "svelte/transition";
    import { createEventDispatcher } from "svelte";

    const dispatch = createEventDispatcher();
</script>

<!-- HTML -->
<div class="modal_wrapper" transition:fade>
    <div class="modal" transition:fly={{y: -150}}>
        <button
            class="close_button"
            on:click={() => {
                // You can choose other name of dispatcher
                dispatch("close");
            }}
        >
            Close
        </button>
        <!--  <slot> Optional fallback </slot>  -->
        <slot/>
        <!--  <slot nam="modal"/> Selected witch teg is fallback -->
    </div>
</div>

<!-- STYLE -->
<style lang="scss">
    .modal_wrapper {
        position: fixed;
        top: 0;
        left: 0;
        right: 0;
        bottom: 0;
        background-color: rgba(0, 0, 0, 0.6);

      .modal {
        background-color: white;
        padding: 20px;
        border-radius: 15px;

        .close_button {
          font-size: 10px;
          margin-bottom: 10px;
        }
      }
    }
</style>