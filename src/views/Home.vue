<template>
  <div class="feature-card">
    <router-link to="/movie/tt3896198">
      <img
          src="https://m.media-amazon.com/images/M/MV5BZmQ5NGFiNWEtMmMyMC00MDdiLTg4YjktOGY5Yzc2MDUxMTE1XkEyXkFqcGdeQXVyNTA4NzY1MzY@._V1_SX300.jpg"
          alt="naruto" class="feature-img">
      <div class="detail">
        <h3>Naruto</h3>
        <p>
          Lorem, ipsum dolor sit amet consectetur adipisicing elit. Eius magni itaque fuga ex deserunt voluptas nihil
          dolorum! Quaerat, iusto libero.
        </p>
      </div>
    </router-link>
  </div>
  <form @submit.prevent="searchMovies()" class="search-box">
    <input type="text" placeholder="What are you looking for?" v-model="search">
    <input type="submit" value="search">
  </form>

  <div class="movies-list">
    <div class="movie" v-for="movie in movies" :key="movie.imdbID">
      <router-link :to="'/movie/' + movie.imdbID" class="movie-link">
        <div class="product-image">
          <img :src="movie.Poster" alt="poster name">
          <div class="type">{{ movie.Type }}</div>
        </div>
        <div class="detail">
          <p class="year">{{ movie.Year }}</p>
          <h3>{{ movie.Title }}</h3>
        </div>
      </router-link>
    </div>
  </div>
</template>

<script>
import {ref} from 'vue'
import env from "@/env";

export default {
  name: 'HomeView',
  setup() {
    const search = ref("")
    const movies = ref([])

    const searchMovies = () => {
      if (search.value != '') {
        fetch(`http://www.omdbapi.com/?apikey=${env.apikey}&s=${search.value}`)
            .then(response => response.json())
            .then(data => {
              movies.value = data.Search
              search.value = ''
              console.log(data)
            })
      }
    }

    return {
      search,
      movies,
      searchMovies
    }
  }
}
</script>

<style lang="scss" scoped>
.feature-card {
  position: relative;

  .feature-img {
    display: block;
    width: 100%;
    height: 300px;
    object-fit: cover;

    position: relative;
    z-index: 0;
  }

  .detail {
    position: absolute;
    left: 0;
    right: 0;
    bottom: 0;
    background-color: rgba(0, 0, 0, 0.6);
    padding: 16px;
    z-index: 1;

    h3 {
      color: #ffffff;
      margin-bottom: 16px;
    }

    p {
      color: #ffffff;
    }
  }
}

.search-box {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  padding: 16px;

  input {
    display: block;
    appearance: none;
    border: none;
    outline: none;
    background: none;

    &[type="text"] {
      width: 100%;
      color: #ffffff;
      background-color: #496583;
      font-size: 20px;
      padding: 10px 16px;
      border-radius: 8px;
      margin-bottom: 15px;
      transition: 0.4s;

      &::placeholder {
        color: #f3f3f3;
      }

      &:focus {
        box-shadow: 0 3px 6px rgba(0, 0, 0, 0.2);
      }
    }

    &[type="submit"] {
      width: 100%;
      max-width: 300px;
      background-color: #42b883;
      padding: 16px;
      border-radius: 8px;
      color: #ffffff;
      font-size: 20px;
      text-transform: uppercase;
      transition: 0.4s;

      &:active {
        background-color: #3b8070;
      }
    }
  }
}

.movies-list {
  display: flex;
  flex-wrap: wrap;
  margin: 0 8px;

  .movie {
    max-width: 50%;
    flex: 1 1 50%;
    padding: 16px 8px;

    .movie-link {
      display: flex;
      flex-direction: column;
      height: 100%;

      .product-image {
        position: relative;
        display: block;

        img {
          display: block;
          width: 100%;
          height: 275px;
          object-fit: cover;
        }

        .type {
          position: absolute;
          padding: 8px 16px;
          background-color: #42b883;
          color: #ffffff;
          bottom: 16px;
          left: 0;
          text-transform: capitalize;
        }
      }

      .detail {
        background-color: #496583;
        padding: 16px 8px;
        flex: 1 1 100%;
        border-radius: 0 0 8px 8px;

        .year {
          color: #aaa;
          font-size: 14px;
        }

        h3 {
          color: #ffffff;
          font-weight: 600;
          font-size: 18px;
        }
      }
    }
  }
}
</style>