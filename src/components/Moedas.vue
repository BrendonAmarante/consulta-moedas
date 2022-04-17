<template>
     <div id ="app"> 
         
         <h1>Moedas</h1><br>
            <input type="text" placeholder="Nome da Moeda" name="author" v-model="name">
            <button type="submit" v-on:click="adicionarMoeda" >Adicionar</button>
            
            <br> <br>
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
                    valor: 0,
                    cadeira: 0
                       
                }
            
            },

            methods: {
                adicionarMoeda(){
                    if ( this.name.trim()===''){
                        return;
                    }
                   
                      axios
                        .get("https://rest.coinapi.io/v1/exchangerate/"+ this.name +"/USD", { headers: {"X-CoinAPI-Key": "9F6E1E03-4C3B-4854-8F1E-63E24538A0A0"}})
                        .then(response => (this.valor = response.data.rate))
                        
                    if (this.valor.length===0){
                            return
                        }
                    
                    this.moeda.push({
                        name: this.name,
                        valor: this.valor
                        
                    });
                    this.cadeira = this.cadeira + 1,
                    this.name ="",
                    this.valor= 0
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