<template>
    <div class="navbar">
        <div @click="$router.push('/')" class="logo">Vue 3</div>
        <div class="navbar__btns">
            <my-button @click="$router.push('/posts')">Посты</my-button>
            <my-button style="margin-left: 20px" @click="$router.push('/about')">О сайте</my-button>
            <my-button style="margin-left: 20px" @click="$router.push('/store')">Store</my-button>
            <my-button style="margin-left: 20px" @click="$router.push('/composition')">Composition</my-button>
        </div>
    </div>
</template>

<script>
export default {

}
</script>

<style scoped>
    .navbar {
        height: 50px;
        background-color: lightgray;
        box-shadow: 2px 2px 4px grey;
        display: flex;
        align-items: center;
        padding: 0 15px;
        justify-content: space-between;
    }
    
    .logo {
        cursor: pointer;
        padding: 10px 15px;
    }
    .logo:hover {
        background-color: antiquewhite;
    }
</style>