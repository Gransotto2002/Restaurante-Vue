<template>
  <Message :msg="msg" v-show="msg"/>
  <div id="burger-table">
    <div>
      <div id="burger-table-heading">
        <div class="order-id">#</div>
        <div>Cliente: </div>
        <div>Pão: </div>
        <div>Carne: </div>
        <div>Opcionais: </div>
        <div>Ações: </div>
      </div>

      <div class="not-burger-msg" v-show="burgers == ''">
        Sem pedidos 😞
      </div>
      <div  v-for="burger in burgers" :key="burger.id"  id="burger-table-rows">
        <div  class="burger-table-row">
          <div class="order-number">{{burger.id}}</div>
          <div>{{burger.nome}}</div>
          <div>{{burger.pao}}</div>
          <div>{{burger.carne}}</div>
          <div>
            <ul>
              <li v-for="(opcional, index) in burger.opcionais" :key="index">{{opcional}}</li>
            </ul>
          </div>
          <div class="acao">
            <select name="status" class="status" @change="updateBurger($event, burger.id)">
              <option value="">Selecione</option>
              <option v-for="s in status" :key="s.id"  :value="s.tipo" :selected="burger.status == s.tipo">{{s.tipo}}</option>
            </select>
            <button class="delete-btn" @click="deleteBurger(burger.id)">Deletar pedido</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>


<script>
import Message from './Message.vue'
export default{
    name: "Dashboard",
    data() {
        return {
            burgers: null,
            burger_id: null,
            status: [],
            msg: ''
        };
    },
    methods: {
        async getPedidos() {
            const req = await fetch("http://localhost:3000/burgers");
            const data = await req.json();
            this.burgers = data;
            // resgatar status
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

            this.getPedidos();

            this.msg = `Pedido ${id} Removido com sucesso !`

            setTimeout(() => {
              this.msg = ''
            }, 3000)
        },
        async updateBurger(event, id) {
            const option = event.target.value;
            const dataJson = JSON.stringify({ status: option });
            const req = await fetch(`http://localhost:3000/burgers/${id}`, {
                method: "PATCH",
                headers: { "Content-Type": "application/json" },
                body: dataJson
            });
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

  #burger-table{
    max-width: 1200px;
    margin: 0 auto;
  }

  #burger-table-heading,
  #burger-table-rows,
  .burger-table-row{
    display: flex;
    flex-wrap: wrap;
  }

  #burger-table-heading{
    font-weight: bold;
    padding: 12px;
    border-bottom: 3px solid #333;
  }

  #burger-table-heading div,
  .burger-table-row div{
    width: 18%;
  }
  
  .burger-table-row{
    width: 100%;
    padding: 12px;
    border-bottom: 1px solid #ccc
  }

  #burger-table-heading .order-id,
  .burger-table-row .order-number{
    width: 5%;
  }

  select{
    padding: 10px 6px;
    margin-right: 12px;
  }


  .delete-btn{
    background: #222;
    color: #fcba03;
    font-weight: bold;
    border: 2px solid #222;
    padding: 10px;
    font-size: 16px;
    margin:0 auto ;
    cursor: pointer;
    transition: .5s;
  }

  .delete-btn:hover{
    background: transparent;
    color: #222;
  }
  
  .burger-table-row .acao{
    width: 23%;
  }

  .not-burger-msg{
    width: 50%;
    text-align: center;
    background: rgb(84, 187, 190);
    margin: 0 auto;
    margin-top: 30px;
    padding: 15px;
    font-weight: bold;
    color: #fff;
    border-radius: 8px;
    border: 2px solid  blue;
  }

</style>