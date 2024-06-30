<template>
    <main class="container buy-item">
        <Table />
        <Cashier />
    </main>
</template>

<script>
import Table from '~/components/Table'
import Cashier from '~/components/Cashier'

export default {
  name: 'Buy',

  components:{
    Table,
    Cashier,
  }
}
</script>

<style scoped>
.buy-item {
    margin-top: 40px;
    width: 100%;
    display: grid;
    grid-template-columns: 3fr 1fr;
    box-sizing: border-box;
}
</style>