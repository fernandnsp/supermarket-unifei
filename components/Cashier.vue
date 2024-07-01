<template>
    <aside class="cashier">
        <div class="inputs-sell">
            <p>Após colocar a quantidade de itens, preencha os campos abaixo e venda.</p>

            <div class="select-style content">
                <div class="select-style">
                    <label for="employee">Funcionário</label>
                    <select
                        class="select-field"
                        v-model="employee.selected"
                        name="employee"
                        id="employee"
                        title="Funcionário"
                    >
                        <option
                            v-for="employee in employee.items"
                            :key="employee.id"
                            :value="employee.id"
                        >{{ employee.name }}</option>
                    </select>
                </div>
                <div class="select-style">
                    <label for="customer">Cliente</label>
                    <select
                        class="select-field"
                        v-model="customer.selected"
                        name="customer"
                        id="customer"
                        title="Cliente"
                    >
                        <option
                            v-for="customer in customer.items"
                            :key="customer.id"
                            :value="customer.id"
                        >{{ customer.name }}</option>
                    </select>
                </div>
            </div>

            <button @click="requestSell()" class="btn-dark">Vender</button>
        </div>
    </aside>
</template>

<script>
    export default {
        name: 'Cashier',
        
        data() {
            return {
                employee: {
                    selected: '',
                    items: []
                },
                customer: {
                    selected: '',
                    items: []
                },
            }
        },

        mounted() {
            this.responseEmployee()
            this.responseCustomer()
        },

        methods: {
            async responseEmployee() {
                try {
                    this.employee.items = await this.$axios.$get(`http://localhost:5000/employee`)
                } catch(err) {
                    console.error(err)
                }
            },
            async responseCustomer() {
                try {
                    this.customer.items = await this.$axios.$get(`http://localhost:5000/customer`)
                } catch(err) {
                    console.error(err)
                }
            },
            async requestSell() {
                try {
                    if (!process.client) return

                    let products = []
                    products = JSON.parse(localStorage.getItem('products'))

                    const productsBuy = products.filter(product => product.buy).map(product => {
                        delete product.name
                        delete product.description
                        delete product.quantity
                        return product
                    })

                    if (!productsBuy.length) {
                        alert('É necessário selecionar a quantidade de produtos que deseja!')
                        return
                    }

                    await this.$axios.$post(
                        `http://localhost:5000/order`, {
                            "customer_id": this.customer.selected,
                            "employee_id": this.employee.selected,
                            "products": productsBuy,
                        }
                    )

                    alert('Vendido com sucesso!')
                    this.clearInputs()
                } catch(err) {
                    console.error(err)
                    alert('Não foi possível concluir a venda')
                }
            },
            clearInputs() {
                this.employee.selected = ''
                this.customer.selected = ''
            }
        }
    }
</script>

<style scoped>
.cashier {
    color: #F2F2F2;
    width: 100%;
    padding: 20px;
    border: 1px solid #1c1c1c;
    border-left: none;
    position: relative;
    background-color: #272727;
}
.inputs-sell {
    display: grid;
    grid-template-columns: 1fr;
    gap: 8px;
    position: sticky;
    top: 20px;
}
.inputs-sell p {
    line-height: 1.5;
}
.select-field {
    padding: 8px;
    font-size: 16px;
    border: 1px solid #ddd;
    border-radius: 4px;
}
.select-style {
    display: grid;
    gap: 16px;
}

.select-style.content {
    gap: 24px;
}
</style>