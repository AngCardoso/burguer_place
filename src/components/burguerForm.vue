<template>
    <div>
        <Message :msg="msg" v-show="msg"/>
        <h1>Monte o seu Hamburger</h1>
        <form action="" id="burguer-form" @submit="createBurguer">
            <div class="input-container">
                <label for="nome" id="opcionais-title">Nome do cliente</label>
                <input type="text" id="nome" name="nome" v-model="nome" placeholder="digite o seu nome">
            </div>
            <div class="input-container">
                <label for="pao"> Escolha o pão</label>
                <select name="pao" id="pao" v-model="pao">
                    <option value=""> escolha o pão</option>
                    <option v-for="pao in paes" :key="pao.id" :value=pao.tipo>{{pao.tipo}}</option>
                </select>
            </div>
            <div class="input-container">
                <label for="carne"> Escolha a carne</label>
                    <select name="carne" id="carne" v-model="carne">
                        <option value="">escolha a carne</option>
                        <option v-for="carne in carnes" :key="carne.id" :value=carne.tipo>{{carne.tipo}}</option>
                    </select>
            </div>
            <div  id="opcionais-container">
                <label for="opcionais" id="opcionais-title">Escolha os opcionais</label>
                <div class='checkbox-container' v-for="opcional in opcionaisData" :key="opcional.id">
                    <input type="checkbox" name="opcionais" v-model="opcionais" :value="opcional.tipo">
                    <span>{{ opcional.tipo}} </span>
                </div>
                
            </div>  
            <div class="input-container">
            <input type="submit" class="submit-btn" value="Criar o meu burguer">
             </div>
        </form>
      
    </div>
</template>
<script>
import Message from './message.vue';
export default{
    name: "BurguerForm",
    components: {
        Message
    },
    data(){
        return{
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
         async getIngredientes(){
            const req = await fetch("http://localhost:3000/ingredientes");
            const data = await req.json();
            this.paes = data.paes
            this.carnes = data.carnes
            this.opcionaisData = data.opcionais
        },
        async createBurguer(e){
            e.preventDefault();
            const Pedido = {
                nome: this.nome, 
                pao: this.pao, 
                carne: this.carne,  
                opcionais: Array.from(this.opcionais), 
                status: 'Solicitado'
                };
            const dataJson = JSON.stringify(Pedido);
            
            const req = await fetch("http://localhost:3000/burgers", 
            {method: 'POST',
            headers: {"Content-Type": 'application/json'},
            body: dataJson});

            const res = await req.json();

            this.msg = `pedido Nº${res.id} realizado com sucesso`;

            setTimeout(() => this.msg = "",3000);

            //limpar campos e cplocar msn
            this.nome="",
            this.carne="",
            this.pao="",
            this.opcionais="";

        }

        
    },
    mounted() {
        this.getIngredientes();
        
        
    }

}
</script>
<style scoped lang="scss">
#burguer-form{
    max-width: 400px;
    margin: 0 auto;
}
.input-container{
    display: flex;
    flex-direction: column;
    margin-bottom: 20px;

    label{
        font-weight: bold;
        margin-bottom: 15px;
        color: #222;
        padding: 5px 10px;
        border-left: 4px solid #fcba03;

        input, select {
            padding: 5px 10px;
            width: 300px;
        }

    }
}

#opcionais-container{
    @extend .input-container;
    flex-direction: row;
    flex-wrap: wrap;

    #opcionais-title{
        width: 100%;
    }

    .checkbox-container{
        display: flex;
        align-items: flex-start;
        width: 50%;
        margin-bottom: 20px;
        
        input{
            width: auto;
        }

        span{
            @extend input;
            margin-left: 6px;
            font-weight: bold;
        } 
    }
}

.submit-btn{
    background: #222;
    color: #fcba03;
    font-weight: bold;
    border: 2px solid #222;
    padding: 10px;
    font-size: 16px;
    margin: 0 auto;
    cursor: pointer;
    transition: 0.5s;
}

.submit-btn:hover{
    background: transparent;
    color: #222;
}
</style>