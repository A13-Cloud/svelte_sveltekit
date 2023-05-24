<!-- JS -->
<script>
    // IMPORTS
    import {fade, fly} from "svelte/transition";

</script>

<!-- HTML -->
<div class="modal_wrapper" transition:fade>
    <div class="modal" transition:fly={{y: -150}}>
        <button>Close</button>
        <!--  <slot> Optional fallback </slot>  -->
        <slot/>
        <!--  <slot nam="modal"/> Selected witch teg is fallback -->
    </div>
</div>

<!-- STYLE -->
<style>
    .modal_wrapper {
        position: fixed;
        top: 0;
        left: 0;
        right: 0;
        bottom: 0;
        background-color: rgba(0, 0, 0, 0.6);
    }

    .modal {
        background-color: white;
        padding: 20px;
        border-radius: 15px;
    }

</style>