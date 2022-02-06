<template>
  <div id="burger-table">
    <Message :msg="msg" v-show="msg" />
    <div>
      <div id="burger-table-heading">
        <div class="order-id">#:</div>
        <div>Cliente:</div>
        <div>Pão:</div>
        <div>Carne:</div>
        <div>Opcionais:</div>
        <div>Ações:</div>
      </div>
    </div>
    <div id="burger-table-rows">
      <div class="burger-table-row" v-for="burger in burgers" :key="burger.id">
        <dig class="order-number">{{ burger.id }}</dig>
        <div>{{ burger.nome }}</div>
        <div>{{ burger.pao }}</div>
        <div>{{ burger.carne }}</div>
        <div>
          <ul>
            <li v-for="(opcional, index) in burger.opcionais" :key="index">
              {{ opcional }}
            </li>
          </ul>
        </div>
        <div>
          <select
            name="status"
            class="status"
            @change="updatedBurger($event, burger.id)"
          >
            <option value="">Selecione</option>
            <option
              v-for="statu in status"
              :key="statu.id"
              :value="statu.tipo"
              :selected="burger.status === statu.tipo"
            >
              {{ statu.tipo }}
            </option>
          </select>
          <button class="delete-btn" @click="deleteBurger(burger.id)">
            Cancelar
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Message from "../components/Message.vue";

export default {
  name: "Dashboard",
  components: {
    Message,
  },
  data() {
    return {
      burgers: null,
      burger_id: null,
      status: [],
      msg: null,
    };
  },
  methods: {
    async getPedidos() {
      const request = await fetch("http://localhost:3000/burgers");
      const data = await request.json();
      this.burgers = data;

      // Resgata os status de pedidos
      this.getStatus();
    },
    async getStatus() {
      const request = await fetch("http://localhost:3000/status");
      const data = await request.json();
      this.status = data;
    },
    async deleteBurger(id) {
      const request = await fetch(`http://localhost:3000/burgers/${id}`, {
        method: "DELETE",
      });

      const response = await request.json();

      this.msg = "Pedido removido com sucesso!";

      setTimeout(() => (this.msg = ""), 3000);

      this.getPedidos();
    },
    async updatedBurger(event, id) {
      const option = event.target.value;

      const dataJson = JSON.stringify({ status: option });

      const request = await fetch(`http://localhost:3000/burgers/${id}`, {
        method: "PATCH",
        headers: { "Content-type": "application/json" },
        body: dataJson,
      });

      const response = await request.json();

      this.msg = `O pedido Nº ${response.id} foi atuaizado para ${response.status}`;

      setTimeout(() => (this.msg = ""), 3000);
    },
  },
  mounted() {
    this.getPedidos();
  },
};
</script>


<style scoped>
#burger-table {
  max-width: 1200px;
  margin: 0 auto;
}

#burger-table-heading,
#burger-table-rows,
.burger-table-row {
  display: flex;
  flex-wrap: wrap;
}

#burger-table-heading {
  font-weight: bold;
  padding: 12px;
  border-bottom: 3px solid #333;
}

#burger-table-heading div,
.burger-table-row div {
  width: 19%;
}

.burger-table-row {
  width: 100%;
  padding: 12px;
  border-bottom: 1px solid #ccc;
}

#burger-table-heading .order-id,
.burger-table-row .order-number {
  width: 5%;
}

select {
  padding: 12px 6px;
  margin-right: 12px;
}

.delete-btn {
  background-color: #222;
  color: #fcba03;
  font-weight: bold;
  border: 2px solid #222;
  border-radius: 5px;
  padding: 10px;
  font-size: 16px;
  margin: 0 auto;
  cursor: pointer;
  transition: 0.5s;
}

.delete-btn:hover {
  background-color: transparent;
  color: #222;
}
</style>