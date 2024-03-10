<script>
  import { createEventDispatcher } from "svelte";

    export let option
    export let index
    const dispatch = createEventDispatcher()

    function removeOption() {
    dispatch('removeFromList', {index: index})
    }
</script>

<div class="option">
    {#if index >= 0}
        <p>{option}</p>
        <button class="remove-option" on:click={() => removeOption()}>X</button>
    {/if}
</div>

<style>
    @keyframes move{
        0%{
            transform: translateX(0);
        }
        50%{
            transform: translateX(-.5rem);
        }
    }
    p{
        margin: 0;
    }
    .option{
        min-width: 3rem;
        min-height: 1rem;
        padding: .5rem 1rem;
        background-color: pink;
        box-shadow: 5px 5px 5px rgb(94, 94, 94);
        border-radius: 1.5rem;
        margin-right: 1rem;
        animation: move 5s ease infinite;
        display: flex;
        align-items: center;
        justify-content: space-between;
    }

    .remove-option{
        color: rgb(255, 0, 43);
        text-align: center;
        cursor: pointer;
        font-size: .8rem;
        background-color: transparent;
        border: none;
        margin-right: -.75rem;
        font-family: 'Trebuchet MS', 'Lucida Sans Unicode', 'Lucida Grande', 'Lucida Sans', Arial, sans-serif;
    }
</style>