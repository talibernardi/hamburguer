<template>
<div id="burguer-table">
</div>
<div id="burguer-table-heading">
    <div class="order-id">#:</div>
    <div>Cliente:</div>
    <div>Pão:</div>
    <div>Carne:</div>
    <div>Opcionais:</div>
    <div>Ações:</div>
  </div>
<div id="burguer-table-rows"> 
    <div class="burguer-table-row" v-for="(burguer, burguerId) in burguers" :key="burguerId">
        <div class="order-number">{{ burguerId }}</div>
        <div>{{ burguer.nome }}</div>
        <div>{{ burguer.pao }}</div>
        <div>{{ burguer.carne }}</div>
        <div>
            <ul>
                <li v-for="(opcional, index) in burguer.opcionais" :key="index">
                  {{ opcional }}
                </li>
            </ul>
        </div>
        <div>
            <select name="status" class="status" @change="updateBurguer($event, burguerId)">
                <option value="">Selecione</option>
                <option v-for="s in status" :key="s.id" :value="s.tipo" :selected="burguer.status == s.tipo">
                    {{ s.tipo }}
                </option>
            </select>
            <button class="delete-btn" @click="deleteBurguer(burguerId)">Cancelar</button>
        </div>
    </div>
 </div>
</template>


<script>
export default {
    name: "Dashboard",
    data() {
        return {
            burguers: null,
            burguer_id: null,
            status: []
        }
    },
    methods: {
        getPedidos() {
            this.burguers = JSON.parse(localStorage.getItem('pedidos'));
            // resgatar status
            this.getStatus();
        },
        async getStatus() {
            const req = await fetch(window.location.origin+'/json/data.json');
            const data = await req.json();
            this.status = data.status;
        },
        deleteBurguer(id) {
            delete this.burguers[id]
            localStorage.setItem('pedidos', JSON.stringify(this.burguers))
            this.getPedidos()
        },
        updateBurguer(e, id) {
            this.burguers[id].status = e.target.value
            localStorage.setItem('pedidos', JSON.stringify(this.burguers))
            this.getPedidos()
        }
    },
    mounted() {
        this.getPedidos();
    }
}
</script>

<style scoped>

#burguer-table {
    max-width: 1200px;
    margin: 0 auto;
}

#burguer-table-heading,
#burguer-table-rows,
.burguer-table-row {
    display: flex;
    flex-wrap: wrap;
}

#burguer-table-heading {
    font-weight: bold;
    padding: 12px;
    border-bottom: 3px solid #333;
}

#burguer-table-heading div,
.burguer-table-row div {
    width: 19%;
}

.burguer-table-row {
    width: 100%;
    padding: 12px;
    border-bottom : 1px solid #ccc;
}

#burguer-table-heading .order-id,
.burguer-table-row .order-number {
    width: 5%
}

select {
    padding: 12px 6px;
    margin-right: 12px;
}

.delete-btn {
    background-color: #222;
    color: #FCBA03;
    font-weight: bold;
    border: 2px solid #222;
    padding: 10px;
    font-size: 16px;
    margin: 0 auto;
    cursor: pointer;
    transition: .5;
}

.delete-btn:hover {
    background-color: transparent;
    color: #222;
}
</style>
