<template>
  <div id="home">

    <!--  Home  -->
    <div id="home-section">
      <div class="home-text-container">
        <h3>Powered by <span class="span-hescsen">HESCSEN</span></h3>
        <h1>Pokedex.</h1>
      </div>
    </div>

    <!--  List  -->
    <div id="list-section">
      <ul class="pokemon-list">
        <li class="pokemon-item" v-for="pokemon in pokemons" :key="pokemon.id">
          <div class="pokedex-container">
            <img :src=pokemon.pictureUrl alt="">
            <div class="pokedex-infos">
              <div class="pokedex-number">#{{ pokemon.id }}</div>
              <div class="pokedex-name">{{ pokemon.name }}</div>
              <ul class="pokedex-type-list">
                <li class="pokedex-type" v-for="type in pokemon.types" :key="type.index">
                  {{ type }}
                </li>
              </ul>
              <div class="pokedex-descr">{{ pokemon.description }}</div>
            </div>
          </div>
        </li>
      </ul>
    </div>

  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'Home',
  data() {
    return {
      pokemons: []
    }
  },

  mounted() {
    const pokemonNumber = 151;

    for (let i = 1; i <= pokemonNumber; i++) {

      axios.get(`https://pokeapi.co/api/v2/pokemon-species/${i}`)
        .then(response => {

          let name;
          let description;
          let types = [];

          // Names
          for (let i = 0; i < response.data.names.length; i++) {
            if (response.data.names[i].language.name === 'fr') {
              name = response.data.names[i].name
              break
            }
          }

          // Description
          for (let i = 0; i < response.data.flavor_text_entries.length; i++) {
            if (response.data.flavor_text_entries[i].language.name === 'fr') {
              description = response.data.flavor_text_entries[i].flavor_text
              break
            }
          }

          axios.get(`https://pokeapi.co/api/v2/pokemon/${i}`)
          .then(response => {

            // Type
            for (let i = 0; i < response.data.types.length; i++) {
                types[i] = response.data.types[i].type.name
              }

            this.pokemons.push({
              id: i.toString().padStart(3, '0'),
              pictureUrl: `https://pokeres.bastionbot.org/images/pokemon/${i}.png`,
              name: name,
              description: description,
              types: types
            })

            this.pokemons.sort((a, b) => {
              return a.id - b.id;
            })
            })
        })
    }
  },

  methods: {

  }
}
</script>

<style lang="scss" scoped>

  #home-section {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
    padding: 0 2rem;

    background: #1a1a1a;
    color: white;

    .home-text-container {
      display: flex;
      flex-direction: column;
      align-items: center;

      h3 {
        margin: 0;
        font-weight: lighter;
        letter-spacing: 0.3rem;
        font-size: 1.2rem;

        .span-hescsen {
          font-weight: bold;
          letter-spacing: 0.1rem;
          color: #ffd100;
        }
      }

      h1 {
        margin: 1.6rem 0;
        text-transform: uppercase;
        font-weight: bold;
        font-size: 10rem;
        letter-spacing: 0.2rem;
      }
    }

    @media screen and (max-width: 1200px) {
      .home-text-container {
        h3 {
          font-size: 1.2rem;
        }

        h1 {
          font-size: 7rem;
        }
      }
    }

    @media screen and (max-width: 750px) {
      .home-text-container {
        h3 {
          font-size: 0.8rem;
        }

        h1 {
          font-size: 3.5rem;
        }

        h2 {
          font-size: 1.2rem;
        }
      }
    }
  }

  .pokemon-item {
    padding: 2rem;
    display: flex;
    justify-content: center;

    background-color: #303030;
    color: white;

    &:nth-child(odd) {
      background-color: #262626;
    }

    .pokedex-container {
      display: flex;
      width: 100%;
      max-width: 1100px;

      img {
        width: 20rem;
        margin-right: 3rem;
      }

      .pokedex-number {
        font-weight: lighter;
        letter-spacing: 0.1rem;
        font-size: 1.6rem;
      }

      .pokedex-name {
        margin: 1rem 0;
        text-transform: uppercase;
        font-weight: bold;
        font-size: 3rem;
        letter-spacing: 0.2rem;
      }

      .pokedex-type-list {
        display: flex;

        .pokedex-type {
          margin-right: 0.4rem;
          padding: 0.2rem 0.4rem;
          text-transform: uppercase;
          font-size: 1rem;
          background: #ffd100;
          color: #1a1a1a;
        }
      }

      .pokedex-descr {
        margin-top: 1.5rem;
        font-weight: lighter;
        letter-spacing: 0.1rem;
        font-size: 1.6rem;
      }
    }

    @media screen and (max-width: 1200px) {
      .pokedex-container {

        img {
          width: 15rem;
        }

        .pokedex-name {
          font-size: 2.5rem;
        }
      }
    }

    @media screen and (max-width: 750px) {
      .pokedex-container {
        flex-direction: column;

        img {
          margin-bottom: 1.5rem;
        }
      }
    }
  }
</style>
