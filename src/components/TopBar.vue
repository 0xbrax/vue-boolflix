<template>
    <div>
        <input type="text" placeholder="Ricerca film" v-model="searchInput" @keyup.enter="$emit('searchInput', searchInput)">
        <button @click="$emit('searchInput', searchInput)">Cerca</button>
    </div>
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

<style lang="scss">

</style>