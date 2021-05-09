<template>
    <div>
        <h2>{{ titulo }}</h2>
    </div>
</template>

<script>
    export default {
        name: "Titulo",
        props: {
            titulo: String
        }
    }
</script>

<style scoped>
    h2 {
        text-align: center;
    }
</style>