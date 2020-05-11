<template>
<view>
    
    <view class="inputContainer">
        <text-input class="input" v-model="pokemon"/>

        <touchable-opacity class="add-btn" :on-press="BuscaPokemon">
            <text class="btn-text">Search</text>
        </touchable-opacity>

    </view>
    
    <view class="contentContainer">
        <view v-if="apistatus">
           <Pokemon :pokeresposta="pokeresposta"/>
        </view>
        <view v-else>
            <text>{{msg}}</text>
        </view>
    </view>
</view>
</template>

<script>
import Pokemon from '../screen/Pokemon/Pokemon';
export default {
    props: {
        data: Object
    },
    components: {
      Pokemon
    },
    data(){
        return{
            pokemon: '',
            apistatus: false,
            msg: 'Search for a pokemon, enter the name or the number.',
            pokeresposta: {},
            carregado: false
        }
    },
    methods:{
        BuscaPokemon(){            
           if(this.pokemon === ''){
                return false
            }            
            this.$http.get(`https://pokeapi.co/api/v2/pokemon/${this.pokemon}`)          
            .then((response) =>{
                    this.pokemon=''
                    this.pokeresposta = response.data
                    this.apistatus = true
                    this.carregado = true            
            })
            .catch((err)=>{
                this.apistatus = false
                this.msg = "Enter a valid pokemon."
                this.carregado = true
            })            
        },
        BuscaRandom(){
            const min = Math.ceil(1)
            const max = Math.floor(807)   
            let random = Math.floor(Math.random() * (max - min)) + min
            this.pokemon =random
            this.BuscaPokemon()
        },
        reseta(){
            this.pokemon=''
            this.apistatus= false
            this.pokeresposta= {}
            this.msg= 'Look for a pokemon that you want.'
        }
    }
};
</script>

<style>
.container {
    background-color: white;
    flex: 1;
}
.inputContainer{
    flex-direction: row;
    justify-content: center;
    padding: 10px;
    background-color:#e9ecef;
    border-color: #e0e0e0;
    border-width: 1px;
}
.contentContainer{
    padding: 2%;
}
.input{
    flex: 1;
    height: 40px;
    background-color: #f5f5f5;
    font-size: 14px;
    color:#333333;
    border-top-left-radius: 5px;
    border-bottom-left-radius: 5px;
}
.add-btn{
    width: 100px;
    height: 40px;
    background-color: #2CF6B3;
    justify-content: center;
    align-items: center;
    border-top-right-radius: 5px;
    border-bottom-right-radius: 5px;
}
.btn-text{
    font-size: 18px;
    font-weight: bold;
}
</style>