<template>
  <h1 v-if="!pokemon">Espere por favor...</h1>
  <div v-else>
    <h1>¿Quién es este pokémon?</h1>
    <!-- img v-bind manda el tipo que se esta asignando -->
    <pokemon-picture 
      :pokemonId="pokemon.id" 
      :showPokemon="showPokemon"
    />
    <!-- opciones -->
    <pokemon-options 
      :pokemons="pokemonArr"
      @selection="checkAnswer"
    />
    <template v-if="showAnswer">
      <h2>{{ message }}</h2>
      <button @click="newGame">
        Nuevo Juego
      </button>
    </template>
  </div>
</template>

<script>
import PokemonPicture from '@/components/PokemonPicture'
import PokemonOptions from '@/components/PokemonOptions'
import getPokemonOptions from '@/helpers/getPokemonOptions';

export default {
  name: 'PokemonPage',
  components:{
    PokemonPicture,
    PokemonOptions
  },
  data(){
    return {
      pokemonArr: [],
      pokemon: null,
      showPokemon: false,
      showAnswer: false,
      message: ''
    }
  },
  methods:{
    async mixPokemonArray(){
      this.pokemonArr = await getPokemonOptions()
      const rndInt = Math.floor(Math.random() * 4) //Numero aleatorio entre 0 y 3
      this.pokemon = this.pokemonArr[rndInt];
    },
    checkAnswer(pokemonId){
      this.showPokemon = true;
      this.showAnswer = true;
      if(pokemonId === this.pokemon.id){
        this.message = `Correcto, es ${this.pokemon.name}` 
      }else{
        this.message = `Oops, era ${this.pokemon.name}` 
      }
    },
    newGame(){
      this.showPokemon = false
      this.showAnswer = false
      this.pokemonArr = []
      this.pokemon = null
      this.mixPokemonArray()
    }
  },
  mounted(){
    this.mixPokemonArray()
  }
}
</script>
