<script>
    import Produktenlijst from "$lib/comps/Produktenlijst.svelte";
    import produkten from "$lib/prods.js";
    let winkelwagen = [];

    const bijwerkenWinkelwagen = (nummer) => {
        console.log(nummer);
        winkelwagen = [produkten[parseInt(nummer)], ...winkelwagen];
    };
</script>

<div class="container">
    <div class="produktenlijst">
        <h2>produktenlijst</h2>
        <Produktenlijst
            on:message={(e) => bijwerkenWinkelwagen(e.detail.text)}
        />
    </div>

    <div class="winkelwagen">
        <button>tonen</button>
        {#if winkelwagen.length > 0}
            {#each winkelwagen as produkt}
                {produkt.naam}
                {produkt.kleur}
            {/each}
        {:else}
            Niets in bestelling
        {/if}
    </div>
</div>

<style>
    .container {
        max-width: 1200px;
        margin: 0 auto;
        display: flex;
    }
    h2 {
        text-align: center;
    }

    .produktenlijst {
        width: 70%;
    }
    .winkelwagen {
        width: 30%;
    }
</style>
