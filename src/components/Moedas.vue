<template>
     <div id ="app"> 
      
         <h1>Moedas</h1><br>
            <input type="text" placeholder="Nome da Moeda" name="author" v-model="name">
            <button type="submit" v-on:click="adicionarMoeda" >Adicionar</button>
             <button type="submit" v-on:click="atualizarMoedas" >Atualizar</button>
            <br> <br>
            <div id="mensagem" v-for="(comment, index) in listaDeMoedas" v-bind:key="index">
               <span class="comment__author">Moeda: <strong>{{comment.name}}</strong></span><br>

               <p>Cotação ${{comment.valor}}</p>             
                <button v-on:click="removerMoeda(index)">Remover Moeda</button>
                <br>
            </div>
    </div>
</template>

<script>

import axios from "axios";
let apikey = "7DC9D883-85FF-44B9-837A-D19EDC4D044C"

export default{
    name: Comment,
    data() {
                return {
                    info: null,
                    moeda: [{ "name": "BTC", "valor": "" }, { "name": "SPG", "valor": "" }, { "name": "SPE", "valor": "" }, { "name": "WEMIX", "valor": "" },{ "name": "BCOIN", "valor": "" }],
                    name: '',
                    valor: '',
                    siglas: "",
                    nome_moeda: ""
                    
                }
            
            },

            methods: {
                mounted(element) {
                   console.log(element)
                    try {
                             axios
                            .get("https://rest.coinapi.io/v1/assets?filter_asset_id="+ element , { headers: {"X-CoinAPI-Key": apikey}})
                            .then (response =>{
                                this.cadeira = [response.data]
                                this.atribuirValores()
})
                            console.log(this.cadeira)
                            
                            this.atribuirValores()
                            
                        } catch (error) {
                            console.log(error)
                        }
                
                },
                atribuirValores(){
                        this.cadeira[0].forEach(api => {
                            
                            this.moeda.forEach(mylist => {
                                
                                if (mylist.name === api.asset_id){
                                    console.log(api,mylist)
                                    mylist.valor = api.price_usd
                                }
                            });
                        });
                },
                atualizarMoedas(){
                    this.nome_moeda = ''
                   this.moeda.forEach(element => {
                       
                       this.nome_moeda = this.nome_moeda + element.name + ","
                       
                    });
                    this.nome_moeda = this.nome_moeda.substring(0, this.nome_moeda.length - 1)
                    this.mounted(this.nome_moeda)
                },
                adicionarMoeda(){
                    if ( this.name.trim()===''){
                        return;
                    }

                    this.moeda.push({
                        name: this.name,
                        valor: this.valor
                        
                    });
                    this.name ="",
                    this.valor= "",
                    this.atualizarMoedas(this.moeda)
                },
                removerMoeda(index){
                    this.moeda.splice(index,1);
                }

            },
            computed: {
                listaDeMoedas(){
                    return this.moeda.map(comment => ({
                        ...comment,
                       
                    }))
                }
            }
}
</script>

<style>
    #mensagem{
        border-color: aqua; border-style: solid;
    }
</style>