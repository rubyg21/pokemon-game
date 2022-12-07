<template>

<h1 v-if="!pokemon"> Espere por favor...</h1>

<!-- <div v-if="pokemon" > -->
<div v-else>
    <h1> Quien es este pokemon? </h1>
    <PokemonPicture
     :pokemon-id="pokemon.id"
     :show-pokemon="showPokemon" />
 
    <PokemonOption  
        :pokemons="pokemonArr"
        @selection-pokemon="checkAnswer"
     /> 
    <template v-if="showAnswer">
      <h2 class="fade-in"> {{ message }} </h2>
     <button @click="newGame">
         Nuevo Juego
     </button>
    </template>
    
<!-- imagenes
opciones -->
</div>
  
</template>
<script>

 import PokemonOption from '@/components/PokemonOption.vue'
 import PokemonPicture from '@/components/PokemonPicture.vue'

 import getPokemonOptions from '@/helpers/getPokemonOptions'

//  console.log(getPokemonOptions());

export default {
 components: { PokemonOption, PokemonPicture  },
 data ( ) {
     return {
         pokemonArr: [],
         pokemon: null,
         showPokemon: false,
         showAnswer: false,
         message: ''
     }
 },
 methods: {
    async mixPokemonArray() {
        this.pokemonArr = await getPokemonOptions()

        const rndInt = Math.floor(Math.random() * 4 )
        // console.log(rndInt);
        this.pokemon = this.pokemonArr[ rndInt ]

     },
     checkAnswer( selectedId ){
     this.showAnswer = true   
     this.showPokemon = true
     if (selectedId === this.pokemon.id) {

         this.message= `Correcto, es ${this.pokemon.name} `
         
     } else {
         
         this.message = `Oops, era ${this.pokemon.name}`
     }
    //  console.log('Pokemon Page llamado', pokemonId);

 },
   newGame(){
       //   this.checkAnswer(selectedId)
    //  this.checkAnswer(),
     this.showPokemon   = false,
     this.showAnswer    = false,
     this.pokemon       = null,
     this.pokemonArr    = [],
     this.mixPokemonArray()
 },
 },
 mounted(){
    //  console.log('mounted')
    this.mixPokemonArray()
   

 },

}
</script>

