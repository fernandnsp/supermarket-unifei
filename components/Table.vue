<template>
    <table>
        <thead>
            <tr>
                <th>Quantidade</th>
                <th>ID</th>
                <th>Nome</th>
                <th>Descrição</th>
                <th>Estoque</th>
            </tr>
        </thead>
        <tbody>
            <tr v-for="item in tableItems" :key="item.id">
                <td class="col-input">
                    <input
                        v-model="item.buy"
                        type="number"
                        min="0"
                        :max="item.quantity"
                    >
                </td>
                <td>{{ item.id }}</td>
                <td>{{ item.name }}</td>
                <td>{{ item.description }}</td>
                <td class="col-input">{{ item.quantity }}</td>
            </tr>
        </tbody>
    </table>
</template>

<script>
    export default {
        name: 'Table',

        data() {
            return {
                tableItems: []
            }
        },

        mounted() {
            this.responseTabItems()
        },

        watch: {
            tableItems: {
                deep: true,
                handler(newVal) {
                    if (!process.client) return
                    localStorage.setItem('products', JSON.stringify(newVal))
                }
            }
        },

        methods: {
            async responseTabItems() {
                try {
                    this.tableItems = await this.$axios.$get(
                        `http://localhost:4000/product`
                    )

                    if (process.client)
                        localStorage.setItem('products', JSON.stringify(this.tableItems))
                } catch(err) {
                    console.error(err)
                    alert(`Não foi possível trazer os dados.`)
                }
            }
        }
    }
</script>

<style scoped>
table {
    width: 100%;
    border-collapse: collapse;
    text-align: center;
}
thead {
    position: sticky;
    top: -1px;
}
th {
    font-weight: 500;
}
th, td {
    color: #F2F2F2;
    border: 1px solid #1c1c1c;
    padding: 8px;
}
th {
    background-color: #272727;
    text-align: center;
}
input[type="number"] {
    width: 100%;
}
.col-input {
    width: 1%;
    white-space: nowrap;
}
</style>