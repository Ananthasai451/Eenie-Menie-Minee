<script>
    import InfoCard from "../lib/InfoCard.svelte";
import Option from "../lib/Option.svelte";
    import Wheel from "../lib/Wheel.svelte";


    let name=''
    let list=[]
    let inputRef

    function addOptionToList(){
        if(name?.length){
        list = [...list, name]
        name=''}
        inputRef.focus()
    }

    function removeFromList(event){
        list = [...list.slice(0,event?.detail?.index), ...list.slice(event?.detail?.index+1)]
    }
</script>

<div class="input">
    <div class="app-title">
        Eenie Menie Minee
    </div>
    <form class="option-form" on:submit|preventDefault={() => addOptionToList()}>
        <input class="option-input" bind:this={inputRef} bind:value={name} placeholder="Enter Option" disabled={list?.length >= 10} />
        <button class="add-button" on:click={() => addOptionToList()}>+</button>
    </form>
    <div class="list-items">
        {#each list as name, index}
		    <Option option={name} index={index} on:removeFromList={removeFromList} />
	    {/each}
    </div>
    {#if list?.length >1 }
        <Wheel {list} />
    {:else}
        <InfoCard />
    {/if}
</div>

<style>
    .app-title{
        font-size: 3rem;
        color: rgb(243, 88, 114);
        font-weight: 900;
        height: 8rem;
    }
    .input{
        width: 100%;
        height: 100%;
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
    }
    .option-form{
        margin-bottom: 2rem;
    }
    .option-input{
        font-size: 2rem;
        border-radius: 16px;
        padding: .5rem;
        outline: none;
        border: 2px transparent solid;
    }
    .option-input:focus{
        border: pink 2px solid;
    }
    .add-button{
        font-size: 2rem;
        cursor: pointer;
        margin-left: 1rem;
        border-radius: 1rem;
    }
    .list-items{
        width:100%;
        display: flex;
        align-items: center;
        justify-content: center;
        flex-wrap: wrap;
        row-gap: 1rem;
    }
</style>