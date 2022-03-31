<template>
  <div class="home">
    <div class="feature-card">
      <router-link to="/movie/tt0409591">
        <img src="https://wallpapersmug.com/large/01e666/Ultra-Instinct-Dragon_Ball-goku.jpg" alt="Goku Ultra-Instinct" class="featured-img" />
        <div class="detail">
            <h3>Goku Ultra-Instinct</h3>
            <p>
              This technique uniquely manifests as a transformation in Son Gokū and is his most powerful form, transcending Super Saiyan Blue. Despite this, Beerus and the other gods recognize the transformation's completed form, hinting that it might not be completely unique to Gokū.[
            </p>
        </div>
      </router-link>
    </div>

    <form @submit.prevent="SearchMovies" class="search-box">
       <input type="text" placeholder="What are you looking for?" v-model="search"/>
       <input type="submit" value="Search"/>
    </form>

  <div class="movies-list">
    <div class="movie" v-for="movie in movies" :key="movie.imdbID">
      <router-link :to="'/movie/'+ movie.imdbID" class="movie-link"> 
        <div class="product-image">
          <img :src="movie.Poster" alt="Movie Poster">
          <div class="type">{{movie.Type}}</div>
        </div>
        <div class="details">
          <div class="year">{{movie.Year}}</div>
          <h3>{{movie.Title}}</h3>
        </div>
      </router-link>
    </div>
  </div>

  </div>
</template>

<script>
// @ is an alias to /src
import {ref} from 'vue';
import env from '@/env'

export default {

    setup() {
    const search = ref("");
    const movies = ref([]);

    const SearchMovies = () =>{
      if(search.value != ""){
          fetch(`http://www.omdbapi.com/?apikey=${env.apiKey}&s=${search.value}`)
          .then(response =>response.json())
          .then(data => {
            movies.value = data.Search;
            search.value = "";
          })

      }
    }

    return{
      search,
      movies,
      SearchMovies
    }
  }
}
</script>

<style lang="scss">
.home {
  .feature-card {
    position:relative;

    .featured-img {
      display: block;
      width: 100%;
      height: 300px;
      object-fit: cover;

      position: relative;
      z-index:0;
    }

    .detail {
      position: absolute;
      left: 0;
      right: 0;
      bottom: 0;
      background-color: rgba(0,0,0,0.6);
      padding:16px;
      z-index:1;

      h3 {
        color: #fff;
        margin-bottom: 16px;

      }
      
      p {
        color:#fff;
      }

    }
  }
  .search-box {
    display: flex;
    flex-direction:column;
    justify-content: center;
    align-items: center;
    padding:16px;

    input { 
      display: block;
      appearance: none;
      border:none;
      outline: none;
      background:none;

      &[type="text"] {
        width:100%;
        color:#FFF;
        background-color: #496583;
        font-size: 20px;
        padding:10px 16px;
        border-radius: 8px;
        margin-bottom: 15px;
        transition: 0.4px;

        &::placeholder {
          color: #f3f3f3;
        }
        &:focus {
          box-shadow: 0px 3px 6px rgba(0,0,0,0.2);
        }
      }

      &[type="submit"]{
        width:100%;
        max-width:300px;
        background-color: #42B883;
        padding:16px;
        border-radius: 8px;
        font-size: 20px;
        color:#fff;
        text-transform: uppercase;
        transition: 0.4s;

        &:active {
          background-color:#3b8070;
        }

      }
    }
  }

  .movies-list{
    display: flex;
    flex-wrap: wrap;
    margin: 0px 8px;

    .movie {
      max-width: 50%;
      flex:1 1 50%;
      padding:16px 8px;

      .movie-link {
        display:flex;
        flex-direction: column;
        height:100%;


        .product-image {
          position:relative;
          display: block;

          img {
            display: block;
            width:100%;
            height:275px;
            object-fit: cover;

          }
          .type {
            position: absolute;
            padding: 8px 16px;
            background-color: #42B883;
            color:#fff;
            bottom:16px;
            left:0px;
            text-transform: capitalize;
          }
        }
        
        .details {
          background-color: #496583;
          padding: 16px 8px;
          flex: 1 1 100%;
          border-radius: 0px 0px 8px 8px;

          .year {
            color:#AAA;
            font-size: 14px;
          }

          h3 {
            color:#FFF;
            font-weight:600;
            font-size:18px;

          }
        }

      }


    }
  }
}

</style>
