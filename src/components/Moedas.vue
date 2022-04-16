<template>
     <div > 
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
export default{
    name: Comment,
    data() {
                return {
                    moeda: [],
                    name: '',
                    valor: ''
                }
            },
            methods: {
                adicionarMoeda(){
                    if ( this.name.trim()===''){
                        return;
                    }
                    this.moeda.push({
                        name: this.name,
                        valor: '7'
                        
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