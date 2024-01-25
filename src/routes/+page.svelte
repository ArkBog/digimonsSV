<script>
  import { onMount } from "svelte";
  import Header from "../components/Header.svelte";
  import {digimons} from "../components/stores";
  import MainPage from "../components/Home.svelte";

  

</script>

<style>
  @import url('https://fonts.googleapis.com/css2?family=Lato:wght@400;700;900&family=Rubik:wght@400;600;900&display=swap');
  
  :root{
    --primary-color: #202A25;
    --secondary-color: #5F4BB6;
    --thrid-color: #5F4BB6;
    --text-color: #26F0F1;
    --transition-details: all .3s linear;
    background-color: var(--primary-color);
    font-family: 'Rubik', sans-serif;
    box-sizing: border-box;
  }
</style>

<Header />
<MainPage    />

