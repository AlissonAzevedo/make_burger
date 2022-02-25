<template>
  <div>
    <Message :msg="msg" v-show="msg" />
  </div>

  <div>
    <form id="burger-form" @submit="createBurger"> <!-- mudar o submit para "createBurger" -->
      <div class="input-container">
        <label for="nome">Nome do Cliente:</label>
        <input
          type="text"
          id="nome"
          v-model="nome"
          name="nome"
          required
          placeholder="Digite o seu Nome"
        />
      </div>

      <div class="input-container">
        <label for="pao">Escolha o Pão:</label>
        <select id="pao" name="pao" v-model="pao" required>
          <option value="">Selecione o seu pão</option>
          <option v-for="pao in paes" :key="pao.id" :value="pao.tipo">
            {{ pao.tipo }}
          </option>
        </select>
      </div>

      <div class="input-container">
        <label for="carne">Escolha a Carne:</label>
        <select id="carne" name="carne" v-model="carne" required>
          <option value="">Selecione o tipo de carne</option>
          <option v-for="carne in carnes" :key="carne.id" :value="carne.tipo">
            {{ carne.tipo }}
          </option>
        </select>
      </div>

      <div class="input-container">
        <label for="bebida">Escolha sua Bebida:</label>
        <select id="bebida" name="bebida" v-model="bebida" required>
          <option value="">Selecione sua bebida</option>
          <option
            v-for="bebida in bebidas"
            :key="bebida.id"
            :value="bebida.tipo"
          >
            {{ bebida.tipo }}
          </option>
        </select>
      </div>

      <div id="opcionais-container" class="input-container">
        <label id="opcionais-label" for="opcionais"
          >Selecione os opcionais:</label
        >
        <div
          class="checkbox-container"
          v-for="opcional in opcionaisData"
          :key="opcional.id"
        >
          <input
            type="checkbox"
            name="opcionais"
            v-model="opcionais"
            :value="opcional.tipo"
          />
          <span>{{ opcional.tipo }}</span>
        </div>
      </div>

      <div class="input-container">
        <input type="submit" class="submit-btn" value="Criar meu Burger" />
      </div>
    </form>
  </div>
</template>

<script>
import Message from "./Message.vue";
export default {
  name: "BurgerForm",
  components: {
    Message,
  },
  data() {
    return {
      paes: null,
      carnes: null,
      bebidas: null,
      opcionaisData: null,
      nome: null,
      pao: null,
      carne: null,
      bebida: null,
      opcionais: [],
      status: "Solicitado",
      msg: null,
    };
  },
  methods: {
    async getIngredientes() {
      const req = await fetch("http://localhost:3000/ingredientes");
      const data = await req.json();
      //console.log(data);
      this.paes = data.paes;
      this.carnes = data.carnes;
      this.bebidas = data.bebidas;
      this.opcionaisData = data.opcionais;
    },
    async createBurger(e) {
      e.preventDefault();
      //console.log("Criou");
      const data = {
        nome: this.nome,
        pao: this.pao,
        carne: this.carne,
        bebida: this.bebida,
        opcionais: Array.from(this.opcionais),
        status: "Solicitado",
      };
        //validate pao or carne or bebida


      const dataJson = JSON.stringify(data);

      const req = await fetch("http://localhost:3000/burgers", {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        body: dataJson,
      });

      const res = await req.json();
      //console.log(resp);
      this.msg = `Pedido N°${res.id} Realizado com Sucesso!`;

      setTimeout(() => {
        this.msg = null;
      }, 3000);

        (this.nome = ""),
        (this.pao = ""),
        (this.carne = ""),
        (this.bebida = ""),
        (this.opcionais = "");
    },
    // async enviarBurger(e){
    //   e.preventDefault();
      

    //   const data = {
    //     nome: this.nome,
    //     pao: this.pao,
    //     carne: this.carne,
    //     bebida: this.bebida,
    //     opcionais: Array.from(this.opcionais),
    //     status: "Solicitado",
    //   };
    //   console.log(data);
    //   window.location = "https://api.whatsapp.com/send/?phone=86988665897&text=nome: "+this.nome+" pao: "+this.pao+" carne: "+this.carne+" bebida: "+this.bebida+" opcionais: "+this.opcionais+" status: "+this.status+ "Numero do Pedido:"+this.id;
     
    // }
  },
  mounted() {
    this.getIngredientes();
  },
};
</script>

<style scoped>
#burger-form {
  max-width: 400px;
  margin: 0 auto;
}
.input-container {
  display: flex;
  flex-direction: column;
  margin-bottom: 20px;
}
label {
  font-weight: bold;
  margin-bottom: 15px;
  color: #222;
  padding: 5px 10px;
  border-left: 4px solid #fcba03;
}
input,
select {
  padding: 5px 10px;
  width: 300px;
}
#opcionais-container {
  flex-direction: row;
  flex-wrap: wrap;
}
#opcionais-title {
  width: 100%;
}
.checkbox-container {
  display: flex;
  align-items: flex-start;
  width: 50%;
  margin-bottom: 20px;
}
.checkbox-container span,
.checkbox-container input {
  width: auto;
}
.checkbox-container span {
  margin-left: 6px;
  font-weight: bold;
}
.submit-btn {
  background-color: #222;
  color: #fcba03;
  font-weight: bold;
  border: 2px solid #222;
  padding: 10px;
  font-size: 16px;
  margin: 0 auto;
  cursor: pointer;
  transition: 0.3s;
  margin-left: 0px;
}
.submit-btn:hover {
  background-color: #fcba03;
  color: #222;
}
</style>