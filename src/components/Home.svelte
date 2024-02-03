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
  let filtersOpen = false;
  let filtersHeaderIcon = "expand_more";
  const expadFilters = () => {
    if (filtersOpen === false) {
      filtersOpen = true;
      filtersHeaderIcon = "expand_less";
    } else {
      filtersOpen = false;
      filtersHeaderIcon = "expand_more";
    }
  };

  let checkedStatus;
  let searcher = "";
  let searchDigimon = () => {
    if (searcher === "") {
      digimonsToDisplay = $digimons;
    } else {
      choosenFilters = [];
      let allActiveFilters = document.querySelectorAll('input[type="checkbox"]');
      allActiveFilters.forEach((checkbox)=>checkbox.checked = false)
      digimonsToDisplay = $digimons.filter((e) => e.name.toLowerCase().includes(searcher));
    }
  };
</script>

<div class="container">
  {#await getData}
    Data is loading
  {:then data}
    <div class="filters">
      <button class="filters-header" on:click={expadFilters}>
        <p>Filters</p>
        <p>
          <span class="material-symbols-outlined"> {filtersHeaderIcon} </span>
        </p>
      </button>
      <div
        class="filters-container"
        style="display: {filtersOpen ? 'flex' : 'none'}"
      >
        <input type="text" bind:value={searcher} on:keyup={searchDigimon} class="searchbar"/>
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
  .filters-container {
    width: 100%;
    flex-direction: column;
    align-items: center;
    justify-content: center;
  }
  .filters {
    margin: 10px auto;
  }
  .filters-header {
    all: unset;
    width: 250px;
    display: flex;
    justify-content: space-around;
    align-items: center;
    background-color: var(--secondary-color);
    border-radius: 5px;
    color: var(--text-color);
  }
  .filters-header:hover {
    cursor: pointer;
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
  .hidden {
    cursor: pointer;
  }
  .hidden:checked + .custom-checkbox {
    background-color: var(--text-color);
    color: var(--primary-color);
  }
  .searchbar{
    all: unset;
    width: 220px;
    height: 100%;
    background-color: var(--secondary-color);
    padding: 5px;
    margin: 20px auto;
    border-radius: 5px;
    color: var(--text-color);

  }
</style>
