<template>
    <nav>
        <div class="container flex just-sp-bw align-ctr">
            <div class="flex align-ctr">
                <a href="#"><h1>Boolflix</h1></a>
                <img :src="require('@/assets/img/popcorn-icon.png')" alt="BoolFlix">
            </div>

            <div>
                <input type="text" placeholder="Ricerca film o serie" v-model="searchInput" @keyup.enter="$emit('searchInput', searchInput)">
                <button @click="$emit('searchInput', searchInput)">Cerca</button>
            </div>
        </div>
    </nav>
</template>

<script>
export default {
    name: 'TopBar',
    data() {
        return {
            searchInput: ''
        }
    }
}
</script>

<style scoped lang="scss">
    nav {
        height: 80px;
        background-color: #0a0a0a;
        .container {
            height: 100%;
        }
    }

    h1 {
        font-size: 40px;
        text-transform: uppercase;
        color: #e50914;
    }

    img {
        height: 40px;
        margin-left: 5px;
    }

    input {
        width: 200px;
        padding: 5px;
        border: 2px solid #e50914;
        border-radius: 5px;
        margin-right: 25px;

        &:focus {
            outline: 2px solid white;
            outline-offset: 1px;
        }
    }

    button {
        background-color: #e50914;
        color: #ffffff;
        padding: 10px 20px;
        border: 2px solid #ffffff;
        border-radius: 5px;

        &:hover {
            cursor: pointer;
            background-color: #eb4b53;
        }

        &:active {
            background-color: #e50914;
        }
    }
</style>