<script>
    export let Produktenlijst;
    import produkten from "$lib/prods.js";

    import { createEventDispatcher } from "svelte";
    const dispatch = createEventDispatcher();
</script>

<div class="overzicht">
    {#each produkten as produkt, i (produkt.naam)}
        <div class="produkt">
            <span>naam: {produkt.naam}</span>
            <span>kleur: {produkt.kleur}</span>
            <button on:click={() => dispatch("message", { text: i })}
                >bestellen</button
            >
        </div>
    {/each}
</div>

<style>
    .overzicht {
        width: 100%;
        display: flex;
        flex-direction: row;
    }
    .produkt {
        width: 25%;
        display: flex;
        flex-direction: column;
        align-items: flex-start;
        justify-content: center;
        margin: 2em;
        border: 1px solid black;
        border-radius: 5px;
        padding: 1em;
    }
    button {
        margin-top: 1em;
    }
</style>
