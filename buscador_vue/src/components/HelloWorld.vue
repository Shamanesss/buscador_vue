<template>
  <div class="buscador">
   <article class="content">
            <h1 class="content__title">Search GitHub users</h1>

            <!-- Search -->
            <form class="search">
                <input type="text" class="search__input" required placeholder="Search GitHub users">
                <input type="submit" class="search__submit" value="Search">
            </form>

            <!-- Result -->
            <!-- Error -->
            <div class="result__error"></div>
        </article>
      </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String
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
  background-color: tomato;
  color: white;
  text-align: center;
  border: 1px solid red;
}

</style>
