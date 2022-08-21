<script>
    import Produktenlijst from "$lib/comps/Produktenlijst.svelte";
    import produkten from "$lib/prods.js";
    import Bestellijst from "$lib/comps/Bestellijst.svelte";
    import winkelwagen from "../store.js";

    
    $:totaalInWinkelwagen = $winkelwagen.length;
    let winkelWagenZichtbaar = true;   
</script>

<div class="container">
    <div class="produktenlijst">
        <h2>produktenlijst</h2>
        <Produktenlijst/>
    </div>

    <div class="winkelwagen">
        <p>{totaalInWinkelwagen} verschillende soorten in winkelwagen</p>
        <button on:click={()=> winkelWagenZichtbaar=!winkelWagenZichtbaar}>tonen</button>
        {#if winkelWagenZichtbaar}
         <Bestellijst />
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
        width: 60%;
    }
    .winkelwagen {
        width: 40%;
    }
</style>
