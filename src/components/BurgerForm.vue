<template>
    <Message :msg="msg" v-show="msg" />
    <div>
        <form id="burger-form"  method="POST" @submit="createBurger">
            <div class="input-container">
                <label for="name">Nome do Cliente:</label>
                <input type="text" id="name" name="name" v-model="name" placeholder="Digite o seu nome:"> 
            </div>
            <div class="input-container">
                <label for="bread">Escolha o pão:</label>
                <select name="bread" id="bread" v-model="bread">
                    <option value="" selected>Selecione o seu pão</option>
                    <option v-for="bread in breads" :key="bread.id" :value="bread.tipo">{{ bread.tipo }}</option>
                </select>
            </div>
            <div class="input-container">
                <label for="beef">Escolha a carne do seu burger:</label>
                <select name="beef" id="beef" v-model="beef">
                    <option value="" selected>Selecione o tipo de carne</option>
                    <option v-for="beef in meat" :key="beef.id" :value="beef.tipo">{{ beef.tipo }}</option>
                </select>
            </div>
            <div class="input-container options-container">
                <label id="options-title" for="options">Selecione os opcionais:</label>
                <div class="checkbox-container" v-for="option in optionsdata" :key="option.id">
                    <input type="checkbox" name="options" v-model="options" :value="option.tipo">
                    <span>{{ option.tipo }}</span>
                </div>
            </div>
            <div class="iput-container">
                <input type="submit" class="submit-btn" value="Criar meu Burger!">
            </div>
        </form>
    </div>
</template>

<script>
import Message from "./Message.vue";
export default{
    name: "BurgerForm",
    data() {
        return {
            breads: null,
            meat: null,
            optionsdata: null,
            name: null,
            bread: null,
            beef: null,
            options: [],
            Status: "Solicitado",
            msg: null
        };
    },
    methods: {
        async getIngredients() {
            const req = await fetch("http://localhost:3000/ingredientes");
            const data = await req.json();
            this.breads = data.paes;
            this.meat = data.carnes;
            this.optionsdata = data.opcionais;
        },
        async createBurger(e) {
            e.preventDefault();
            const data = {
                name: this.name,
                beef: this.beef,
                bread: this.bread,
                options: Array.from(this.options),
                status: "Solicitado"
            };
            const dataJson = JSON.stringify(data);
            const req = await fetch("http://localhost:3000/burgers", {
                method: "POST",
                headers: { "Content-Type": "application/json" },
                body: dataJson
            });
            const res = await req.json();

            this.msg = 'Pedido Nº '+res.id+' realizado com sucesso'

            setTimeout(() => this.msg = "", 5000);

            this.beef = "";
            this.name = "";
            this.bread = "";
            this.options = [];
        }
    },
    mounted() {
        this.getIngredients();
    },
    components: { 
        Message 
    }
}
</script>

<style scoped>
    #burger-form{
        max-width: 400px;
        margin: 0 auto;
    }

    .input-container{
        display: flex;
        flex-direction: column;
        margin-bottom: 20px;
    }
    
    label {
        font-weight: bold;
        margin-bottom: 15px;
        color: #222;
        padding: 5px 10px;
        border-left: 4px solid #FCBA03;
    }

    input, select {
        padding: 5px 10px;
        width: 300px;
    }

    #options-container {
        flex-direction: row;
        flex-wrap: wrap;
    }

    .options-title {
        width: 100%;
    }

    .checkbox-container{
        display: flex;
        align-items: flex-start;
        width: 50%;
        margin-bottom: 20px;
    }

    .checkbox-container span, .checkbox-container input{
        width: auto;
    }

    .checkbox-container span{
        margin-left: 6px;
        font-weight: bold;
    }

    .submit-btn{
        background-color: #222;
        color: #FCBA03;
        font-weight: bold;
        border: 2px solid #222;
        padding: 10px;
        font-size: 16px;
        margin: 0 auto;
        cursor: pointer;
        transition: .50s;
    }

    .submit-btn:hover{
        background-color: transparent;
        color: #222;
    }
</style>
