<template>
    <div>
        <p>Componente de Mensagem</p>
        <form id="burger-form" @submit="createBurger">
            <div class="input-container">
                <label for="nome">Nome do cliente:</label>
                <input type="text" placeholder="Digite o seu nome" v-model="nome" name="nome" id="nome">
            </div>
            <div class="input-container">
                <label for="pao">Escolha o pão:</label>
                <select name="pao" id="pao" v-model="pao">
                    <option value="">Selecione o seu pão</option>
                    <option v-for="pao in paes" :key="pao.id" :value="pao.tipo">{{ pao.tipo }}</option>
                </select>
            </div>
            <div class="input-container">
                <label for="carne">Escolha a carne:</label>
                <select name="carne" id="carne" v-model="carne">
                    <option value="">Selecione a carne</option>
                    <option v-for="carne in carnes" :key="carne.id" :value="carne.tipo">{{ carne.tipo }}</option>
                </select>
            </div>
            <div class="input-container">
                <label for="opcional">Selecione os opcionais:</label><br>
                <div class="checkbox-container">
                    <div v-for="opcional in opcionaisData" :key="opcional.id" class="checkbox-individual">
                        <input type="checkbox" class="input-checkbox" v-model="opcionais" :value="opcional.tipo">
                        <span>{{ opcional.tipo }}</span>
                    </div>
                </div>
            </div>
            <div>
                <input type="submit" class="submit-btn" value="Criar meu burger!">
            </div>
        </form>
    </div>
</template>

<script>
export default {
    name: "BurgerForm",
    data() {
        return {
            paes: null,
            carnes: null,
            opcionaisData: null,
            nome: null,
            pao: null,
            carne: null,
            opcionais: [],
            msg: null
        }
    },
    methods: {
        async getIngredientes() {
            const req = await fetch("http://localhost:3000/ingredientes");
            const data = await req.json();

            this.paes = data.paes;
            this.carnes = data.carnes;
            this.opcionaisData = data.opcionais;
        },
        async createBurger(e) {
            e.preventDefault();
            const data = {
                nome: this.nome,
                carne: this.carne,
                pao: this.pao,
                opcionais: Array.from(this.opcionais),
                status: "Solicitado"
            }
            const dataJson = JSON.stringify(data);

            const req = await fetch("http://localhost:3000/burgers", {
                method: "POST",
                headers: { "Content-Type": "application/json" },
                body: dataJson
            });
            const res = await req.json();
            console.log(res);

            // colocar mensagem de sistema

            // limpar mensagem

            // limpar os campos
            this.nome = null;
            this.carne = null;
            this.pao = null;
            this.opcionais = []
        }
    },
    mounted() {
        this.getIngredientes();
    }
}
</script>

<style scoped>
    #burger-form {
        max-width: 400px;
        margin: 0 auto;
    }

    .input-container {
        display: flex;
        flex-direction: column;
        margin-bottom: 20px
    }

    label {
        font-weight: bold;
        margin-bottom: 15px;
        color: #222;
        padding: 5px 10px;
        border-left: 4px solid #FCBA03
    }

    input, select {
        padding: 5px 10px;
        width: 300px
    }

    .checkbox-container {
        width: 300px
    }

    .checkbox-individual {
        display: inline;
        margin-left: 7px
    }

    .checkbox-individual input {
        width: 30px
    }

    .submit-btn {
        background-color: #222;
        color: #FCBA03;
        font-weight: bold;
        border: 2px solid #222;
        padding: 10px;
        font-size: 16px;
        margin: 0 auto;
        cursor: pointer;
        transition: .5s
    }

    .submit-btn:hover {
        background-color: transparent;
        color: #222
    }
</style>