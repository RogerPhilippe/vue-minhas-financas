<template>
    <div>

        <div class="py-3 text-center">
            <img :src="images.sample" />
            <h1 class="h2 py-2">{{ title }}</h1>
        </div>
        <div class="text-center">
            <button type="button" class="btn btn-primary">Importar</button>
            <button type="button" class="btn btn-primary">Apagar</button>
        </div>

        <p v-if="notificationMustShow" class="alert alert-primary my-3">{{notification}}</p>

        <div class="form-group">
            <label for="item">Item</label>
            <input type="text" v-model="financa.description" id="item" class="form-control" placeholder="Item" required/>
        </div>

        <div class="form-group">
            <label for="data">Data</label>
            <input type="date" v-model="financa.date" id="data" class="form-control" value="2020-03-19" required/>
        </div>

        <div class="form-row">
            <div class="form-group col-6">
                <label for="quantidade">Quantidade</label>
                <input type="number" v-model="financa.quant" id="quantidade" class="form-control" min="1" step="1" required/>
            </div>
            <div class="form-group col-6">
                <label for="valor">Valor</label>
                <input type="number" v-model="financa.value" name="" id="valor" class="form-control" min="1.00" step="0.10" required/>
            </div>
        </div>

        <input type="submit" v-on:click="addFinanca" name="Enviar" id="" class="btn btn-primary"/>

        <div class="py-3" id="financasView"></div>

        <div class="list-group">
            <div class="list-group-item" v-for="(item, index) in listaFinancas" v-bind:key="index">
                <table class="table">
                    <thead>
                        <tr>
                            <th data-col="description">Item</th>
                            <th data-col="date">Data</th>
                            <th data-col="quant">Quantidade</th>
                            <th data-col="value">Valor</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr>
                            <td>{{item.description}}</td>
                            <td>{{item.date}}</td>
                            <td>{{item.quant}}</td>
                            <td>{{item.value}}</td>
                        </tr>
                    </tbody>
                </table>
            </div>
        </div>

    </div>
</template>

<script>

export default {
    data() {
        return {
            title: 'Minhas finanças',
            notification: 'Finança adicionada',
            notificationMustShow: false,            
            financa: {
                description: '',
                date: '',
                value: '',
                quant: ''
            },
            listaFinancas: [],
            images: {
                sample: require('@/assets/logo.svg' )
            }
        }
    },
    methods: {
        addFinanca() {
            const financa = {
                description: this.financa.description,
                date: this.financa.date,
                value: this.financa.value,
                quant: this.financa.quant
            }

            this.listaFinancas.push(financa)

            this.notificationMustShow = true

            this.financa.description = '',
            this.financa.date = '',
            this.financa.value = '',
            this.financa.quant = ''
        }
    }
}

</script>

<style>
img {
    width: 150px;
    height: 150px;
}
</style>
