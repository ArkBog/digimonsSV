<script>
    const menu = ["Menu", "Favourites", "About us", "Contact"]
</script>

<style>
    .container{
        width: 100%;
        background-color: red;
        color: yellow;
        height: 200px;
    }
    .menu{
        display: flex;
        flex-direction: column;
    }
</style>

<div class="container">
    <div class="menu">
        {#each menu as option }
            <div>{option}</div>
        {/each}
    </div>
</div>