<template>
    <div id="burguer-table">
        <Message :msg="msg" v-show="msg"/>
        <div>
            <div id="burguer-table-heading">
                <div class="order-id">#</div>
                <div>Cliente:</div>
                <div>Pão:</div>
                <div>Carne:</div>
                <div>Opcionais:</div>
                <div>Ações:</div>
            </div>
        </div>
        <div id="burguer-table-rows">
            <div class="burguer-table-row" v-for="burguer in burguers" :key="burguer.id">
                <div class="order-number">
                    {{burguer.id}}
                </div>
                <div>{{burguer.nome}}</div>
                <div>{{burguer.pao}}</div>
                <div>{{burguer.carne}}</div>
                <div>
                    <ul>
                        <li v-for="(opcionais, index) in burguer.opcionais" :key="index">{{opcionais}}</li>
                    </ul>
                </div>
                <div>
                    <select name="status" class="status" @change="updateBurguer($event, burguer.id)">
                        <option v-for="stato in status" :key="stato.id" :value="stato.tipo" :selected="burguer.status == stato.tipo">{{ stato.tipo }}</option>
                    </select>
                    <button class="delete-btn" @click="deleteBurguer(burguer.id)">Cancelar pedido</button>
                </div>
            </div>

        </div>
    </div>
</template>
<script>
import Message from './message.vue';
export default{
    name: "Dasboard",
    components:{Message},
    data(){
       return {
            burguers: null,
            burguer_id: null,
            status:[],
            msg: null
        }
    },
    methods:{
        async getPedidos(){
            const req = await fetch('http://localhost:3000/burgers');
            const data = await req.json();
            this.burguers = data;
            
        },

        async getStatus(){
            const req = await fetch('http://localhost:3000/status');
            const data = await req.json();
            this.status = data;
        },

        async deleteBurguer(id){
            const req = await fetch(`http://localhost:3000/burgers/${id}`,{
                method: "DELETE"
            });
            const res = await req.json();
            this.getPedidos();

            this.msg = `pedido Nº${id} removido com sucesso`;
            setTimeout(() => this.msg = "",3000);
        },
        async updateBurguer(event, id){

            const option = event.target.value;

            const dataJson = JSON.stringify({status: option});

            const req = await fetch(`http://localhost:3000/burgers/${id}`,{
                method: "PATCH",
                headers:{"Content-Type": "application-json"},
                body: dataJson
            });

            this.msg = `pedido Nº${id} foi atualizado para ${option}`;

            setTimeout(() => this.msg = "",3000);

            const res = await req.json();
            
        }

    },
    mounted(){
        this.getPedidos();
        this.getStatus();
    }
}
</script>
<style lang="scss" scoped>

#burguer-table{
    max-width: 1200px;
    margin:0 auto;
    #burguer-table-heading,
    #burguer-table-rows,
    .burguer-table-row{
        display: flex;
        flex-wrap: wrap;
    }

    #burguer-table-heading{
        font-weight: bold;
        padding: 12px;
        border-bottom:3px solid #333;
    }
    #burguer-table-heading div,
    .burguer-table-row div{
        width: 19%
    }


    .burguer-table-row{
        width: 100%;
        padding: 12px;
        border-bottom: 1px solid #ccc
        
    }

    #burguer-table-heading .order-id,
    .burguer-table-row .order-number{
        width: 5%;
    }

    select{
        padding: 12px 6px;
    }

    .delete-btn{
        background: #222;
        color: #fcba03;
        font-weight: bold;
        border: 2px solid #222;
        padding: 10px;
        font-size: 16px;
        margin: 0 auto;
        cursor: pointer;
        transition: 0.5
    }
    .delete-btn:hover{
        background: transparent;
        color: #fcba03;
    }
}
</style>