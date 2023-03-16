<template>
  <div class="buscador">

    <div class="favorites">
      <div class="favorite" v-for="[, favorite] in favorites" :key="favorite.id">
        <a href="#">
          <img :src="favorite.avatar_url" alt="" class="favorite__avatar">
        </a>

      </div>

    </div>
   <article class="content">
            <h1 class="content__title">{{ msg }}</h1>

            <!-- Search -->
            <form class="search" v-on:submit.prevent="doSearch">
                <input v-model="search" type="text" class="search__input" required placeholder="Search GitHub users">
                <input type="submit" class="search__submit" value="Search">
            </form>

            <!-- Result -->
            <!-- v-show hace lo mismo que v-if a nivel visual pero es un poco mas costoso a la hora de renderizar v-if -->
            <!-- <div class="result" v-show="result" > -->
            <div class="result" v-if="result" >
               <a href="#" v-if="isfavorite" class="result__toggle-favorite" @click="delFavorite">Eliminar de Favoritos ⭐️</a>
               <a href="#" v-else class="result__toggle-favorite" @click="addFavorite">Añadir a Favoritos ⭐️</a>
              <h2 class="result__name">{{ result.login }}</h2>
              <img v-bind:src="result.avatar_url" v-bind:alt="result.name" class="result__avatar">
              <p class="result__bio">{{ result.bio }}</p>
              <br>
              <a v-bind:href="result.blog" target="_blank" class="result__blog">{{ result.html_url }}</a>



            </div>
            <p v-else>Esperando a que introduzcas un usuario.......</p>
            <!-- Error -->
            <div class="result__error" v-if="error">{{error}}</div>
        </article>
      </div>
</template>

<script>
import { ref, computed }  from 'vue'
export default {
  name: 'HelloWorld',

  setup() {
    const API ="https://api.github.com/users/"
     let search = ref(null)
     let result = ref(null)
     let error = ref(null)
     let favorites = ref(new Map())
      const doSearch= async()=>{
        result.value= error.value = null
        try {
        
          const response = await fetch(API + search.value)
          if(!response.ok) throw new Error('Usuario no encontrado')
          console.log(response)
          const data= await response.json()
          console.log(data)
          result.value = data

        }catch (e) {
        error.value = e
      } finally{
        search.value =null
      }
      
     }

     const addFavorite = function () {
        favorites.value.set(result.value.id, result.value)
      }
      const delFavorite = function () {
        favorites.value.delete(result.value.id)
      }
      const isfavorite = computed(() => {
  return favorites.value.has(result.value.id)
})
        

    return{
      msg : ref("Nombre de usuario"),
      search,
       result,
       error,
       favorites,
       addFavorite,
      doSearch,
      delFavorite,
      isfavorite
    }
  }



}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.buscador {
  font-family: sans-serif;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: rgb(227, 226, 230);
  font-size: 1.3rem;
  padding: 17rem;
}

/* Content */
.content {
  background-color: #42b983;
  width: 768px;
  max-width: 768px;
  padding: 3rem;
  box-shadow: 2px 2px 3px gray;
  border-radius: 1rem;
}

.content__title {
  margin: 1rem;
  text-align: center;
}
.search {
  display: flex;
  margin-bottom: 2.5rem;
}

.search__input {
  flex: 1;
  padding: 1rem;
  font-size: 1rem;
}
.result__error {
  padding: 0.3rem;
  background-color: black;
  color: white;
  text-align: center;
  border: 1px solid black;
}
.result {
  position: relative;
  background-color: purple;
  border-radius: 0.3rem;
  box-shadow: 2px 2px 3px gray;
  color: white;
  padding: 2.5rem;
  display: grid;
  gap: 1rem;
  grid-template-areas:
    "name name"
    "avatar bio";
}
.result__toggle-favorite {
  position: absolute;
  top: 0.3rem;
  right: 0.3rem;
  border: none;
  color: white;
  text-decoration: none;
  padding: 0.4rem;
}

.result__name {
  grid-area: name;
  margin: 0.4rem 0;
}
.result__avatar {
  grid-area: avatar;
  max-width: 6rem;
  height: auto;
  border-radius: 1rem;
}

.result__bio {
  grid-area: bio;
  margin: 0;
}

.result__blog {
  grid-area: blog;
  color: goldenrod;
}
.favorites {
  position: fixed;
  top: 5rem;
  left: 2rem;
  width: 100%;
  display: flex;
}

.favorite {
  transition: transform 0.3s ease-out;
}

.favorite__avatar {
  height: 5rem;
  margin: 0.3rem;
}

.favorite--selected {
  transform: scale(1.3);
}

/* Transitions */
.v-enter-active,
.v-leave-active {
  transition: opacity 0.5s ease;
}

.v-enter-from,
.v-leave-to {
  opacity: 0;
}

.list-move, /* apply transition to moving elements */
.list-enter-active,
.list-leave-active {
  transition: all 0.5s ease;
}

.list-enter-from,
.list-leave-to {
  opacity: 0;
  transform: translateX(30px);
}

/* ensure leaving items are taken out of layout flow so that moving
   animations can be calculated correctly. */
.list-leave-active {
  position: absolute;
}

</style>
