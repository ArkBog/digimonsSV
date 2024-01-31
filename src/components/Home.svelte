<script lang>
  // @ts-nocheck

  import { digimons, favourites } from "./stores";
  import { goto } from "$app/navigation";

  let digimonsToDisplay = [];

  const getData = fetch("https://digimon-api.vercel.app/api/digimon")
    .then((response) => response.json())
    .then((data) => {
      $digimons = data;
      digimonsToDisplay = $digimons;
    })
    .catch((error) => console.error(error));

  const addToFavourites = (digimon) => {
    if ($favourites.includes(digimon)) {
      let index = $favourites.findIndex((e) => e === digimon);
      $favourites.splice(index, 1);
    } else {
      $favourites.push(digimon);
    }
    console.log($favourites);
  };

  const navigateToComponent = () => goto("/details");

  const filters = () => {
    let filtersArray = [];
    for (let i = 0; i < $digimons.length; i++) {
      if (
        filtersArray.includes($digimons[i].level) &&
        filtersArray.length != 0
      ) {
        continue;
      } else {
        filtersArray.push($digimons[i].level);
      }
    }
    return filtersArray;
  };

  let choosenFilters = [];

  let runFilter = (arg) => {
    if (choosenFilters.includes(arg)) {
      let index = choosenFilters.findIndex((e) => e === arg);
      choosenFilters.splice(index, 1);
      console.log(choosenFilters);
    } else {
      choosenFilters.push(arg);
      console.log(choosenFilters);
    }
    digimonsToDisplay = $digimons.filter((e) =>
      choosenFilters.includes(e.level)
    );
    if (choosenFilters.length === 0) {
      digimonsToDisplay = $digimons;
    }
  };
</script>

<div class="container">
  {#await getData}
    Data is loading
  {:then data}
    <div class="filters">
      {#each filters() as filter}
        <div class="filter">
          <input
            type="checkbox"
            value={filter}
            class="hidden"
            on:change={(event) => runFilter(event.target.value)}
          />
          <label for="filter" class="custom-checkbox">{filter}</label>
        </div>
      {/each}
    </div>
    {#each digimonsToDisplay as digimon}
      <div class="card">
        <h1 class="card-name">{digimon.name}</h1>
        <div class="card-img">
          <img
            class="digimon-img"
            src={digimon.img}
            alt={digimon.name}
            on:click={() => navigateToComponent()}
          />
          <button class="add-to-fav" on:click={() => addToFavourites(digimon)}
            >Add to favourites</button
          >
        </div>
        <p class="card-text">{digimon.level}</p>
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
  .card {
    border-radius: 2rem;
    width: 300px;
    margin: 1rem auto;
    background-color: var(--secondary-color);
    overflow: hidden;
  }
  h1,
  .card-text {
    margin: auto;
    text-align: center;
    color: var(--text-color);
    font-weight: 600;
    padding: 5px 0;
  }
  .card-img {
    position: relative;
    overflow: hidden;
  }
  .digimon-img:hover {
    transition: var(--transition-details);
    transform: scale(1.2);
    cursor: pointer;
  }
  .add-to-fav {
    position: absolute;
    top: 0px;
    right: 0px;
  }
  .filter {
    position: relative;
    width: 200px;
    height: 30px;
    margin: 5px;
    color: var(--text-color);
  }
  .custom-checkbox {
    display: block;
    background-color: var(--secondary-color);
    border-radius: 5px;
    width: 100%;
    height: 100%;
    text-align: center;
    line-height: 1.7;
  }
  .hidden {
    position: absolute;
    width: 100%;
    height: 100%;
    top: 0;
    left: 0;
    z-index: 1;
    opacity: 0;
  }
  .hidden:checked + .custom-checkbox {
    background-color: red;
  }
</style>
