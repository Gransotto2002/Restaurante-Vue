<template>
	<div>	
		<div>
			<form id="burger-form" @submit="createBurger">
				<Message :msg="msg"  v-show="msg"/>
				<div class="input-container">
					<label for="nome">Nome do cliente</label>
					<input type="text" id="nome" name="nome" v-model="nome" placeholder="Digite seu nome">
				</div>
				<div class="input-container">
					<label for="pao">Escolha o tipo de pão</label>
					<select name="pao" id="pao" v-model="pao">
						<option value="">Selecione seu pão</option>
						<option v-for="pao in paes" :key="pao.id" :value="pao.tipo">
							{{pao.tipo}}
						</option>
					</select>
				</div>
				<div class="input-container">
					<label for="carne">Escolha sua carne</label>
					<select name="carne" id="carne" v-model="carne">
						<option>Selecione sua carne</option>
						<option v-for="carne in carnes" :key="carne.id" :value="carne.tipo">
							{{carne.tipo}}
						</option>
					</select>
				</div>
				<div id="opcionais-container" class="input-container">
					<label id="opcionais-title" for="opcionais">Escolha seus opcionais</label>
					<div v-for="opcional in opcionaisdata" :key="opcional.id" class="checkbox-container">
						<input type="checkbox" name="opcionais" v-model="opcionais" :value="opcional.tipo">
						<span>{{opcional.tipo}}</span>
					</div>
				</div>

				<div class="input-container">
					<input type="submit" value="Criar meu Burger" class="submit-btn">
				</div>
			</form>
		</div>
	</div>
</template>

<script>
import Message from './Message.vue';
export default {
    name: "Form",
    data() {
        return {
            paes: null,
            carnes: null,
            opcionaisdata: null,
            nome: null,
            pao: null,
            carne: null,
            opcionais: [],
            msg: null
        };
    },
    methods: {
        async getIngredientes() {
            const req = await fetch("http://localhost:3000/ingredientes");
            const data = await req.json();
            this.paes = data.paes;
            this.carnes = data.carnes;
            this.opcionaisdata = data.opcionais;
        },

        async createBurger(e) {
            e.preventDefault();
            const data = {
                nome: this.nome,
                carne: this.carne,
                pao: this.pao,
                opcionais: Array.from(this.opcionais),
                status: "Solicitado"
            };


            const dataJson = JSON.stringify(data);
            const req = await fetch("http://localhost:3000/burgers", {
                method: "POST",
                headers: { "Content-Type": "application/json" },
                body: dataJson
            });


            const res = await req.json();
            /* Limpar campos */
            this.pao = "";
            this.nome = "";
            this.carne = "";
            this.opcionais = [];

						this.msg = `Pedido Nº ${res.id} Realizado com Sucesso`

						setTimeout(() => {
							this.msg = ''
						}, 5000)


        }
    },
    mounted() {
        this.getIngredientes();
    },
    components: { Message }
}
</script>


<style scoped>
	#burger-form{
		display: flex;
		flex-direction: column;
		align-items: center;
		max-width: 400px;
		margin: 0 auto;
	}

	.input-container{
		display: flex;
		flex-direction: column;
		margin-bottom: 20px;
	}

	label{
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

	#opcionais-container{
		width: 300px;
		flex-direction: row;
		flex-wrap: wrap;
	}

	#opcionais-title{
		width: 100%;
	}

	.checkbox-container{
		display: flex;
		align-items: flex-start;
		width:  50%;
		margin-bottom: 20px;
	}

	.checkbox-container span,
	.checkbox-container input{
		width: auto;
	}

	.checkbox-container span{
		margin-left: 6px;
		font-weight: bold;
	}

	.submit-btn{
		background: #222;
		color: #fcba03;
		font-weight: bold;
		border: 2px solid #222;
		font-size: 16px;
		margin: 0 auto;
		cursor: pointer;
		transition: .5s;
	}

	.submit-btn:hover{
		background-color: transparent;
		color: #222;
	}

</style>>

