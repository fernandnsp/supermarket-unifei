<template>
    <header class="header">
        <div class="container header-content">
            <h1>Supermercado</h1>
            <nuxt-link :to="route.link">{{ route.name }}</nuxt-link>
        </div>
    </header>
</template>

<script>
    export default {
        name: 'BaseHeader',

        props: {
            route: {
                type: Object,
                default: () => ({
                    link: '/cadastro',
                    name: 'Cadastrar'
                })
            }
        }
    }
</script>

<style scoped>
.header {
    background-color: #6FCDC2;
}
.header-content {
    display: flex;
    align-items: center;
    justify-content: space-between;
}
</style>