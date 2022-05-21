<template>
    <p>Componente Mensagem</p>
    <div>
        <form id="burger-form">
            <div class="input-container">
                <label for="name">Nome do Cliente:</label>
                <input type="text" id="name" name="name" v-model="name" placeholder="Digite o seu nome:"> 
            </div>
            <div class="input-container">
                <label for="bread">Escolha o pão:</label>
                <select name="bread" id="bread" v-model="bread">
                    <option value="" selected>Selecione o seu pão</option>
                </select>
            </div>
            <div class="input-container">
                <label for="beef">Escolha a carne do seu burger:</label>
                <select name="beef" id="beef" v-model="beef">
                    <option value="" selected>Selecione o tipo de carne</option>
                </select>
            </div>
            <div class="input-container options-container">
                <label id="options-title" for="options">Selecione os opcionais:</label>
                <div class="checkbox-container">
                    <input type="checkbox" name="options" v-model="options" value="salame">
                    <span>Salame</span>
                </div>
            </div>
            <div class="iput-container">
                <input type="submit" class="submit-btn" value="Criar meu Burger!">
            </div>
        </form>
    </div>
</template>

<script>
export default{
    name: 'BurgerForm'
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
