<template>
    <div>
        <div class="Menu-contairner">
            <NuxtLink to="/" class="menu-item">
                <div class="menu-item-contairner">
                    <div class="menu-item-content">List</div>
                </div>
            </NuxtLink>
            <NuxtLink :to="{ name: 'CreateEditPage', params: { id: 0}}" class="menu-item">
                <div  class="menu-item-contairner">
                    <div class="menu-item-content">New</div>
                </div>
            </NuxtLink>
            <NuxtLink to="/SearchPage" class="menu-item">
                <div class="menu-item-contairner">
                    <div class="menu-item-content">Search</div>
                </div>
            </NuxtLink>
        </div>
    </div>
</template>
<script>
    export default {
    }
</script>

<style>
/*Menu*/
    #App-Menu{
        width: 17%;
        min-height: 100%;
    }
    .Menu-contairner{
        width: 100%;
    }
    .menu-item{
        width: 100%;
        height: 50px;
        display: flex;
        justify-content: center;
        color: black;
        text-decoration: none;
    }
    .menu-item:hover{
        background-color: #ccc;
        color: black;
        text-decoration: none;
    }
    .menu-item-contairner{
        width: 80%;
        height: 100%;
        display: flex;
        align-items: center;
    }
    .menu-item-contairner:hover{
        cursor: pointer;
    }
    .menu-item-content{
        font-family: Arial;
        font-size: 17px;
        width: 60px;
        padding-left: 20px;
    }

</style>