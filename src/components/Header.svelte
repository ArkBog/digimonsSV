<script>
    const menu = ["Menu", "Favourites", "About us", "Contact"];

    let menuIsOpen = false;
    
    const displayMenu = () => {
        if(menuIsOpen === false){
            menuIsOpen = true;
        }
        else{
            menuIsOpen = false;
        }
    }
</script>

<style>
    .container{
        width: 100%;
        background-color: var(--secondary-color);
        color: var(--text-color);
        height: 100px;
        display: flex;
        justify-content: space-between;
        align-items:center;
    }
    .menu{
        display: flex;
        flex-direction: column;
        position: absolute;
        width: 100%;
        height: 100%;
        z-index: 5;
        top: 0;
        left: 0;
        background-color: var(--secondary-color);
        text-align: center;
    }
    .option{
        width: 100%;
        margin: 50px 0 0px 0;
        padding: 5px 0px 5px 0px;
        font-size: 2rem;
        text-transform: uppercase;
        letter-spacing: 2px;
        
    }
    .option:hover{
        cursor: pointer;
        color: var(--primary-color);
        transition: all 1s ease-out;
    }
    .close-menu{
        font-size: 50px;
        font-weight: 600;
        position: absolute;
        top: 10px;
        right: 10px;
    }
    .close-menu:hover{
        cursor: pointer;
        color: var(--primary-color);
        transition: all 1s ease-out;
    }
    .logo{
        font-size: 50px;
        font-weight: 900;
        padding: 1rem;
        text-transform: uppercase;
        letter-spacing: 2px;
    }
    .hamburger-menu:hover{
        cursor: pointer;
        
    }
    .material-symbols-outlined{
        font-size: 50px;
        color: var(--text-color);
        font-weight: 900;
        padding: 5px;
    }
    .material-symbols-outlined:hover{
        color: var(--primary-color);
        transition: all 1s ease-out;
    }
    
</style>

<div class="container">
    <div class="logo">
        DIGIMONS
    </div>
    <div class="menu" style="display: {menuIsOpen ? 'flex' : 'none'}">
        <div class="close-menu" on:click={() => displayMenu()}>X</div>
        {#each menu as option }
            <div class="option">{option}</div>
        {/each}
    </div>
    <div class="hamburger-menu" on:click={() => displayMenu()}>
        <span class="material-symbols-outlined">
            menu
        </span>
    </div>
</div>