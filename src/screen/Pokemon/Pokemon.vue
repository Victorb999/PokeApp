<template>
    <view class="contentpoke">
        <text class="h1">
             #{{pokeresposta.id}} {{pokeresposta.name}}
        </text>
        <view class="containerpoke">
            <image
            :style="{width: 150, height: 150}"
            :source="{uri:fotourl}"
            />        
        </view>   
             
            <view class="infos" :class='tipo[0].type.name'>
            <text class="h2 branco">{{alcunha}}</text>  

            <view class="poke-descricao">
                <text class="h2">Description</text>

                <text v-if="descricao">{{descricao}}</text>
            </view> 

            <view class="poke-extrainfo">
                <text class="h3" v-if="especie.habitat">Habitat: {{especie.habitat.name}}</text>
                <text class="h3" v-if="especie.shape">Body: {{especie.shape.name}}</text>

                <text class="h3">Height: {{pokeresposta.height/10}} m</text>
                <text class="h3">Weight: {{pokeresposta.weight/10}} kg</text>            
            </view>

            <view class="poke-tipos">
                <text class="h2">Type</text>
                <view class="tipos">
                    <view v-for='(tipos,index) in tipo' :key='index'  class="texto-tipo">
                        
                            <text class="badge" :class='tipo[index].type.name'>{{tipo[index].type.name}}  </text> 
                            
                    </view>
                </view>
            </view>
            
    </view>
</template>

<script>
export default {
    props:{
        pokeresposta:Object
    },
    data(){
        return{
            tipo : [],
            fotourl : "https://toyama.com.br/images/imagens.png",
            especie: [],
            evolucao: [],
            evolucoes: [],
            evolucoesURL : [],
            descricao: '',
            alcunha:'',
            formas:[],
            carregado: false            
        }
    },
    mounted(){
       this.setaImg()     
    },
    watch:{
        pokeresposta:{
            immediate: true,
            handler() {                
                this.setaImg()
                this.evolucoes = []
                this.evolucoesURL = []
                this.descricao = ''
                this.alcunha = ''
                this.carregado = false
                this.buscaespecie(this.pokeresposta.species.url)               
            }
        },
    },
    methods:{
        setaImg(){
            let numero 
            numero = this.pokeresposta.id < 999 ? ('000' + this.pokeresposta.id).substr(-3) : this.pokeresposta.id
            this.tipo = this.pokeresposta.types 
            if(numero < 1000){
                this.fotourl = `https://assets.pokemon.com/assets/cms2/img/pokedex/full/${numero}.png`
            }            
        },
        async  buscaespecie(url){
            await this.$http.get(url)          
            .then((response) =>{                    
                this.especie = response.data 

                 response.data.genera.map((alc,index)=>{
                    
                    if(alc.language.name === 'en'){
                        if(this.alcunha === ''){
                            this.alcunha =  alc.genus
                        }
                    }
                })

                response.data.flavor_text_entries.map((desc,index)=>{
                    
                    if(desc.language.name === 'en'){
                        if(this.descricao === ''){
                            this.descricao = this.descricao + desc.flavor_text
                        }
                    }
                })

                //this.descricao = response.data.flavor_text_entries[6].flavor_text
                //console.log(response.data.evolution_chain.url)
                if(response.data.evolution_chain.url){
                    this.buscaEvolucao(response.data.evolution_chain.url)
                }
                this.carregado = true
            //console.log(response)
            })
            .catch((err)=>{
                this.especie = {'erro' : err}
            })
        },
       async buscaEvolucao(url){
            await this.$http.get(url)          
            .then((response) =>{                    
                this.evolucao = response.data
            //console.log(response)
            })
            .catch((err)=>{
                this.evolucao = {'erro' : err}
            })
        },
        setaEvolucao(){
           
            if(this.evolucao){
           //return true
                 this.evolucoes = [...this.evolucoes,this.evolucao.chain.species.name]
                 this.evolucoesURL = [...this.evolucoesURL,this.evolucao.chain.species.url]
                 //console.log(this.evolucao.chain.evolves_to)
                 this.evolucao.chain.evolves_to.map((ev,index) =>{
                    
                     this.evolucoes = [...this.evolucoes,ev.species.name]
                     this.evolucoesURL = [...this.evolucoesURL,ev.species.url]
                     // ev.evolves_to[index].evolves_to.map((ev2,index2) =>{                         
                    //     this.evolucoes = [...this.evolucoes,ev2.evolves_to[index2].species.name]
                    // })
                    if(ev.evolves_to.length > 0){
                        this.evolucoes = [...this.evolucoes,ev.evolves_to[index].species.name]
                        this.evolucoesURL = [...this.evolucoesURL,ev.evolves_to[index].species.url]
                    }
                 })
            }
        },
        variedadevalido(){
            if(typeof this.especie.varieties  === 'undefined'){
                return false
            }
            else if(this.especie.varieties.length === 1){
                return false
            } 
            else{
                return true
            }
        }
    }
}
</script>

<style>
.h1{
    font-size: 18px;
    color:#333;
    font-weight: 900;
}
.h2{
    font-size: 16px;
    color:#333;
    font-weight: 700;
}
.h3{
    font-size: 14px;
    color:#333;
    font-weight: 500;
}
.branco{
    color: #fff;
}
.contentpoke{
    flex: 1;
}
.containerpoke{
    background-color: #f5f5f5;
    padding: 10px;
    margin:10px;
    border-radius:5px;
    flex-direction: row;
    justify-content: center;
}
.poke-descricao,.poke-extrainfo,.poke-tipos{
    background-color: #fff;
    padding: 3px;
    margin:3px;
    border-radius:5px; 
}
.infos{
    padding: 10px;
    margin:10px;
    border-radius:5px;
}
.badge{
    border-radius:5px;
    width: auto;
    color:#fff;
    padding: 2px;
    margin: 2px;
    text-align: center;
}
.normal{
    background-color: #d3d3af;
}
.fighting{
    background-color: #d56723;
}
.flying{
    background-color: #a1d1f8;
}
.poison{
    background-color: #7fa8c9;
}
.ground{
    background-color: #7c7e29;
}
.rock{
    background-color: #a38c21;
}
.bug{
    background-color: #729f3f;
}
.ghost{
    background-color: #7b62a3;
}
.steel{
    background-color: #9eb7b8;
}
.fire{
    background-color: #fd7d24;
}
.water{
    background-color: #4592c4;
}
.grass{
    background-color: #9bcc50;
}
.electric{
    background-color: #eed535;
}
.psychic{
    background-color: #f366b9;
}
.ice{
    background-color: #51c4e7;
}
.dragon{
    background-color: #3d167c;
}
.dark{
    background-color: #303030;
}
.fairy{
    background-color: #fdb9e9;
}
.unknown{
    background-color: #000;
}
.shadow{
    background-color: #333;
}
</style>