<template>
    <main class="container signup">
        <aside class="type-signup signup-style">
            <label for="signup">Quem você quer cadastrar?</label>
            <select
                v-model="selectedType"
                name="signup"
                id="signup"
                title="Tipo de cadastro"
            >
                <option value="Cliente">Cliente</option>
                <option value="Funcionário">Funcionário</option>
                <option value="Produto">Produto</option>
            </select>
        </aside>

        <aside class=signup-style>
            <p class="signup-info">Cadastro de {{ selectedType }}</p>

            <div class="signup-inputs" v-if="selectedType !== 'Produto'">
                <div v-for="input in inputs" :key="input.label" class="input-field">
                    <label for="name">{{ input.label }}</label>
                    <input
                        v-model="input.value"
                        :type="input.type"
                        :id="input.id"
                        :name="input.id"
                        :placeholder="input.placeholder"
                    >
                </div>
            </div>

            <div class="signup-inputs" v-else>
                <div v-for="input in inputsProduct" :key="input.label" class="input-field">
                    <label for="name">{{ input.label }}</label>
                    <input
                        v-model="input.value"
                        :type="input.type"
                        :id="input.id"
                        :name="input.id"
                        :placeholder="input.placeholder"
                    >
                </div>
            </div>

            <button @click="requestSignup()" class="btn-dark">Cadastrar</button>
        </aside>
    </main>
</template>

<script>
    export default {
        name: 'SignUp',

        data() {
            return {
                selectedType: 'Funcionário',
                inputs: [
                    {
                        value: '', label: 'Nome', type: 'text',
                        id: 'name', placeholder: 'John Doe',
                    },
                    {
                        value: '', label: 'Endereço', type: 'text',
                        id: 'address', placeholder: 'Rua Dos bobos, 0, bairro ali',
                    },
                    {
                        value: '', label: 'Celular', type: 'number',
                        id: 'phone', placeholder: '35999999999',
                    },
                ],
                inputsProduct: [
                    {
                        value: '', label: 'Nome', type: 'text',
                        id: 'name', placeholder: 'Mesa',
                    },
                    {
                        value: '', label: 'Descrição', type: 'text',
                        id: 'description', placeholder: 'Mesa de madeira',
                    },
                    {
                        value: '', label: 'Quantidade', type: 'number',
                        id: 'quantity', placeholder: '10',
                    },
                ],
            }
        },

        watch: {
            selectedType() {
                this.clearInputs()
            }
        },

        methods: {
            async requestSignup() {
                const typeRequest = {
                    'Cliente': 'customer',
                    'Funcionário': 'employee',
                    'Produto': 'product',
                }
                const payload = {}

                if (this.selectedType !== 'Produto') {
                    payload.name = this.inputs[0].value
                    payload.phone = this.inputs[1].value
                    payload.address = this.inputs[2].value
                } else {
                    payload.name = this.inputsProduct[0].value
                    payload.description = this.inputsProduct[1].value
                    payload.quantity = this.inputsProduct[2].value
                }

                try {
                    await this.$axios.$post(
                        `http://localhost:5000/${typeRequest[this.selectedType]}`, JSON.stringify(payload)
                    )

                    alert(`${this.selectedType} cadastrado com sucesso!`)
                    this.clearInputs()
                } catch(err) {
                    console.error(err)
                    alert(`Não foi possível fazer o cadastrado.`)
                }
            },
            clearInputs() {
                this.inputs.forEach(input => {
                    input.value = ''
                })
                this.inputsProduct.forEach(input => {
                    input.value = ''
                })
            }
        }
    }
</script>

<style scoped>
.signup {
    max-width: 600px;
    width: 100%;
    margin-top: 40px;
    background-color: #272727;
}

.type-signup {
    display: flex;
    flex-direction: column;
    gap: 16px;
}

.signup-style {
    color: #F2F2F2;
    width: 100%;
    padding: 20px;
    border: 1px solid #1c1c1c;
}

.signup-info {
    margin-top: 0;
    margin-bottom: 24px;
    font-size: 20px;
}

.signup-inputs {
    display: grid;
    gap: 16px;
}

#signup {
    padding: 8px;
    font-size: 16px;
    border: 1px solid #ddd;
    border-radius: 4px;
}

#phone::-webkit-inner-spin-button{
    -webkit-appearance: none;
    appearance: none;
}
</style>