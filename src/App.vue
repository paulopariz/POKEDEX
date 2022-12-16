<template>
  <v-app>
    <v-container>
      <v-container>
        <v-text-field 
        v-model="search" 
        label="Pesquisar" 
        placeholder="Charmander" 
        color></v-text-field>

        {{ search }}

        <v-row>
          <v-col cols="2" 
          v-for="pokemon in filtered_pokemons" 
          :key="pokemon.name">
          
            <v-card @click="show_pokemon(get_id(pokemon))">
              <v-container>
                <v-row class="mx-0 d-flex justify-center">
                  <img 
                  :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${get_id(
                      pokemon
                      )}.png`" 
                      :alt="pokemon.name" 
                      width="80%" 
                      />
                </v-row>
                <h2 class="text-center"> {{ get_name(pokemon) }}</h2>
              </v-container>
            </v-card>
          </v-col>
        </v-row>
      </v-container>
    </v-container>


    <v-dialog v-model="show_dialog" width="900">
      <v-card v-if="selected_pokemon">
        <v-container>
          <v-row>
            <v-col cols="4">
              <img 
                  :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${selected_pokemon.id}.png`" 
                      :alt="selected_pokemon.name" 
                      width="80%" 
                      />
            </v-col>
          </v-row>

          {{ selected_pokemon }}
        </v-container>
      </v-card>
    </v-dialog>
  </v-app>
</template>

<script>
  import axios from "axios";

  export default {
    name: 'App',

    components: {},

    data: () => {
      return {
        pokemons: [],
        search: "",
        show_dialog: false,
        selected_pokemon: null,
      };
    },

    mounted() {
      axios
        .get("https://pokeapi.co/api/v2/pokemon?limit=151")
        .then((response) => {
          this.pokemons = response.data.results;
        });
    },

    methods: {
      get_id(pokemon) {
        return pokemon.url.split("/")[6];
      },
      get_name(pokemon) {
        return pokemon.name.charAt(0).toUpperCase() + pokemon.name.slice(1);
      },
      show_pokemon(id) {
        axios.get(`https://pokeapi.co/api/v2/pokemon/${id}`).then((response) => {
            this.selected_pokemon = response.data;
            this.show_dialog = !this.show_dialog;
          });
      },
    },
 
    computed: {
      filtered_pokemons() {
        return this.pokemons.filter((item) => {
          return item.name.includes(this.search);
        });
      }
    }
  };
</script>

<style>
  #app {
    background-color: rgb(15, 11, 45);
  }
</style>