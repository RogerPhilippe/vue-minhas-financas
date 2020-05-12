<template>
    <div>

        <div class="py-3 text-center">
            <img :src="images.sample" />
            <h1 class="h2 py-2">{{ title }}</h1>
        </div>
        <div class="text-center">
            <button type="button" v-on:click="request" class="btn btn-primary">Importar</button>
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

        <div class="list-group">
            <div class="list-group-item">
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
                        <tr v-for="(financa, index) in listaFinancas" v-bind:key="index">
                            <td>{{financa.item}}</td>
                            <td>{{financa.data}}</td>
                            <td>{{financa.total}}</td>
                            <td>{{financa.valor}}</td>
                        </tr>
                    </tbody>
                </table>
            </div>
        </div>

    </div>
</template>

<script>

import { FinancaService } from '../services/FinancaService'
import { Financa } from '../models/Financa'
import { DateHelper } from '../helpers/DateHelper'

export default {

    data() {
        return {
            title: 'Minhas finanças',
            notification: '',
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
            const financa = new Financa(
                this.financa.description,
                DateHelper.textoParaData(this.financa.date),
                this.financa.quant,
                this.financa.value
            )

            this.listaFinancas.push(financa)

            this.notificationMustShow = true
            this.notification = 'Finança adicionada'

            this.financa.description = '',
            this.financa.date = '',
            this.financa.value = '',
            this.financa.quant = ''
        },
        request() {
            console.log('Request')
            new FinancaService().getFinancasSemana()
            .then(financas => { 
                this.notificationMustShow = true
                this.notification = 'Finanças importadas'
                financas.map(
                    financa => {
                        this.listaFinancas.push(financa)
                    }
                )
            })
            .catch(error => { 
                console.log(error) 
            })
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
