<script>
    // overzicht van de produkten en bij drukken op bestellen wordt het produkt in de besellijst geplaats
    import produkten from "$lib/prods.js";

    import { createEventDispatcher } from "svelte";
    import winkelwagen from "../../store.js";
    $:cart=$winkelwagen;

    //op basis van aantal de prijs per stuk bepalen
    const totaalPrijsUitrekenen = (staffellijst, aantal) =>
    {
            let i=0;
            while (parseInt(staffellijst[i])>parseInt(aantal) || (i==staffellijst.length))
                i++;
                    
            i=staffellijst.length-i-1;
            //if (i>staffellijst.length-1) i=staffellijst.length-1;
            return (i)
         }


    const verwerkenBestelling = info =>{

    
        // opzoeken product in produktenlijst
        const pos = produkten.findIndex(e=>e.naam===info.naam);
        // prijzen uit de prijslijst halen omgekeerde lijst maken om later de totaal prijs te kunnen berekenen
        const prijzen = produkten[pos].stuks;
        let staffellijst=[];
            prijzen.forEach((e)=>staffellijst.unshift(e.aantal));
        const aantal=(produkten[pos].stuks.findIndex(e=>e.aantal==info.aantal));
        //kijken of er al in de winkelwagen zit
        const posinWagen = cart.findIndex(e=>e.naam===info.naam);
        
        if (posinWagen<0){
        //toevoegen aan winkelwagen als nog niet aanwezig
        let toevoeging=produkten[pos];     
        toevoeging.aantal=produkten[pos].stuks[aantal].aantal;
        //
      
        toevoeging.prijsTotaal = produkten[pos].stuks[totaalPrijsUitrekenen(staffellijst, toevoeging.aantal)].prijs;            
        cart = [...cart,toevoeging];
        } else {

         //als produkt al in winkelwagen zit -> aantal bijwerken
        let prodInWagen=cart[posinWagen];
        prodInWagen.aantal=parseInt(prodInWagen.aantal)+parseInt(produkten[pos].stuks[aantal].aantal);
        prodInWagen.prijsTotaal = produkten[pos].stuks[totaalPrijsUitrekenen(staffellijst, prodInWagen.aantal)].prijs;
        cart[posinWagen]=prodInWagen;
        };

        
        winkelwagen.set(cart);


    }
</script>

<div class="row">
    {#each produkten as produkt (produkt.naam)}
        <div class="produkt col">
            <div class="w100 col">
                <div class="w100 row"><div class='w33'>Naam:</div><div class='w33'> {produkt.naam}</div></div>
                <div class="w100 row"><div class='w33'>Kleur: </div><div class='w33'>{produkt.kleur}</div></div>
            </div>

            <div class='w100 col'>
                <div class='row'>
                    <div class='w33'>stuks</div><div class='w33'>prijs</div>
                </div>
            {#each produkt.stuks as staffel (staffel.aantal)}
                <div class='row'>
                    <div class='w33'>{staffel.aantal}</div>
                    <div class='w33'>{staffel.prijs}</div>
                    <button class='w33' on:click={() => { 
                            verwerkenBestelling({naam:produkt.naam, aantal: staffel.aantal })
                            }}
                    >bestellen</button
                >
                </div>
            {/each}
            </div>
        </div>
    {/each}
</div>

<style>
   
    .produkt {
        width: 25%;
        align-items: flex-start;
        justify-content: center;
        margin: 2em;
        border: 1px solid black;
        border-radius: 5px;
        padding: 1em;
    }


    
</style>
