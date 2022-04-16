<template>
     <div id ="app"> 
         <h1>Moedas</h1><br>
            
            <input type="text" placeholder="Nome da Moeda" name="author" v-model="name">
            <button type="submit" v-on:click="adicionarMoeda" >Adicionar</button>
            <br> <br>
            <div id="mensagem">
               <span class="comment__author">Moeda: <strong>BTC</strong></span><br>  
               <p>{{valorBTC}}</p>
            </div>
            <div id="mensagem" v-for="(comment, index) in listaDeMoedas" v-bind:key="index">
               <span class="comment__author">Moeda: <strong>{{comment.name}}</strong></span><br>  
               <p>{{comment.valor}}</p>             
                <button v-on:click="removerMoeda(index)">Remover Moeda</button>
                <br>
            </div>
    </div>
</template>

<script>
import axios from "axios";
                  

export default{
    name: Comment,
    data() {
                return {
                    info: null,
                    moeda: [],
                    name: '',
                    valor: '',
                    valorBTC: "",
                                                 
                }
            },mounted() {
                axios
                .get("https://rest.coinapi.io/v1/exchangerate/BTC/USD", { headers: {"X-CoinAPI-Key": "82AD1ED4-EA0A-4F77-AFA2-7B894C2AA95C"}})
                .then(response => (this.valorBTC = response.data.rate))
            },
            methods: {
                adicionarMoeda(){
                    if ( this.name.trim()===''){
                        return;
                    }
                    this.moeda.push({
                        name: this.name,
                        valor: ''
                        
                    });
                    this.name ="";
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