<script lang>
// @ts-nocheck

  import { digimons, favourites } from "./stores";
  import {goto} from "$app/navigation";

  const getData = fetch("https://digimon-api.vercel.app/api/digimon")
    .then((response) => response.json())
    .then((data) => {
      $digimons = data;
    })
    .catch((error) => console.error(error));

    const addToFavourites = (digimon) => {
      if($favourites.includes(digimon)){
        let index = $favourites.findIndex((e)=> e === digimon);
        $favourites.splice(index, 1)
      }
      else{
        $favourites.push(digimon);
      }
      console.log($favourites) 
    }

  const navigateToComponent = () => goto('/details');

</script>

<div class="container">
  {#await getData}
    Data is loading
  {:then data}
    {#each $digimons as digimon}
      <div class="card" >
        <h1 class="card-name" >{digimon.name}</h1>
        <img src={digimon.img} alt={digimon.name} on:click={() => navigateToComponent()}/>
        <p class="card-text">{digimon.level}</p>
        <button on:click={() => addToFavourites(digimon)}>Add to favourites</button>
      </div>
    {/each}
  {:catch error}
    Something went wrong
  {/await}
</div>

<style>
  .container {
    width: 100%;
    display: flex;
    flex-direction: column;
  }
</style>
