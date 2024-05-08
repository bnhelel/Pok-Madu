<template>
    <div id="pokemon">
      <h1>{{ upperCaseName }}</h1>
      
      <div class="card">
        <div class="card-image">
          <figure>
            <img :src="currentImg" alt="Imagem do Pokémon" />
          </figure>
        </div>
        <div class="card-content">
          <div class="media">
            <div class="media-content">
              <p class="title is-4">{{ num }} - {{ upperCaseName }}</p>
              <p class="subtitle is-6">{{ pokemon.type }}</p>
            </div>
          </div>
          <div class="content">
            <button class="button is-medium is-fullwidth" @click="mudarSprite">Mudar sprite</button>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  
  export default {
    props: {
      num: Number,
      name: String,
      url: String
    },
    data() {
      return {
        isFront: true,
        currentImg: '',
        pokemon: {
          type: '',
          front: '',
          back: ''
        }
      };
    },
    created() {
      // Realiza a requisição HTTP utilizando axios
      axios.get(this.url)
        .then(res => {
          // Atribui os dados obtidos da API às propriedades do Pokémon
          this.pokemon.type = res.data.types[0].type.name;
          this.pokemon.front = res.data.sprites.front_default;
          this.pokemon.back = res.data.sprites.back_default;
          this.currentImg = this.pokemon.front;
          console.log(this.pokemon); // Verifica os dados do Pokémon no console
        })
        .catch(error => {
          console.error('Erro ao carregar dados do Pokémon:', error);
        });
    },
    computed: {
      upperCaseName() {
        // Método computado para transformar o nome em maiúsculas
        return this.name ? this.capitalizeFirstLetter(this.name) : '';
      }
    },
    methods: {
      capitalizeFirstLetter(value) {
        // Método para capitalizar a primeira letra da string
        if (!value) return '';
        return value.charAt(0).toUpperCase() + value.slice(1);
      },
      mudarSprite() {
        // Método para alternar entre as imagens do Pokémon
        if (this.isFront) {
          this.isFront = false;
          this.currentImg = this.pokemon.back;
        } else {
          this.isFront = true;
          this.currentImg = this.pokemon.front;
        }
      }
    }
  };
  </script>
  
  <style>
  #pokemon {
    margin-top: 2%;
  }
  </style>
  