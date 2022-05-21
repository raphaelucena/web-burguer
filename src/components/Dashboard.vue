<template>
    <div>
        <Message :msg="msg" v-show="msg"/>
        <table id="burger-table">
            <thead id="burger-table-heading">
                <tr>
                    <th>#:</th>
                    <th>Nome:</th>
                    <th>Pão:</th>
                    <th>Carne:</th>
                    <th>Opcionais:</th>
                    <th>Ações:</th>
                </tr>
            </thead>
            <tbody v-for="burger in burgers" :key="burger.id">
                <tr>
                    <th>{{burger.id}}</th>
                    <td>{{burger.name}}</td>
                    <td>{{burger.bread}}</td>
                    <td>{{burger.beef}}</td>
                    <td>
                        <ul>
                            <li v-for="(options, index) in burger.options" :key="index">{{options}}</li>
                        </ul>
                    </td>
                    <td>
                        <select name="status" class="status" @change="updateBurger($event, burger.id)">
                            <option v-for="s in status" :key="s.id" :value="s.tipo" :selected="burger.status == s.tipo">{{s.tipo}}</option>
                        </select>
                        <button class="delete-btn" @click="deleteBurger(burger.id)" >Cancelar</button>
                    </td>
                </tr>
            </tbody>
        </table>
    </div>
</template>

<script>
import Message from './Message.vue';
export default{
    name: "Dashboard",
    data() {
        return {
            burgers: null,
            burger_id: null,
            status: [],
            msg: null
        };
    },
    methods: {
        async getPedidos() {
            const req = await fetch("http://localhost:3000/burgers");
            const data = await req.json();
            this.burgers = data;
            this.getStatus();
        },
        async getStatus() {
            const req = await fetch("http://localhost:3000/status");
            const data = await req.json();
            this.status = data;
        },
        async deleteBurger(id) {
            const req = await fetch(`http://localhost:3000/burgers/${id}`, {
                method: "DELETE"
            });
            const res = await req.json();

            this.msg = `Pedido removido com sucesso!`;

            setTimeout(() => this.msg = "", 5000);

            this.getPedidos();
        },
        async updateBurger(event, id) {
            const option = event.target.value;
            const dataJson = JSON.stringify({ status: option });
            const req = await fetch(`http://localhost:3000/burgers/${id}`, {
                method: "PATCH",
                headers: { "Content-Type": "application/json" },
                body: dataJson
            });
            const res = await req.json();

            this.msg = `O Pedido Nº ${res.id} foi atualizado para ${res.status}!`;

            setTimeout(() => this.msg = "", 5000);
        }
    },
    mounted() {
        this.getPedidos();
    },
    components: { 
        Message 
    }
}

</script>

<style scoped>

    #burger-table {
        max-width: 1200px;
        margin: 0 auto;
    }
    
    #burger-table thead tr th {
        font-weight: bold;
        padding: 12px;
        border-bottom: 3px solid #333;
    }

    #burger-table thead tr th, #burger-table tbody tr th, #burger-table tbody tr td, #burger-table tbody tr td ul{
        width: 19%;
        text-align: center;
        
    }

    #burger-table tbody tr th, #burger-table tbody tr td{
        padding: 12px;
        border-bottom: 1px solid #CCC;
    }

    #burger-table thead tr th:nth-child(1), #burger-table tbody tr th {
        width: 5%;
    }

    select {
        padding: 12px 6px;
        margin-right: 12px;
    }

    .delete-btn{
        background-color: #222;
        color: #fcba03;
        font-weight: bold;
        border: 2px solid #222;
        padding: 10px;
        font-size: 16px;
        margin: 0 auto;
        cursor: pointer;
        transition: .5s;
    }

    .delete-btn:hover{
        background-color: transparent;
        color: #222;
    }

</style>
