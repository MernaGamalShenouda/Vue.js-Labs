<template>
  <div class="container">
    <h1 class="mt-5 mb-4">Recipe Details</h1>
    <div class="card" v-if="prd">
      <img :src="prd.image" class="card-img-top" alt="Recipe Image">
      <div class="card-body">
        <h2 class="card-title">Name</h2>
        <h3>{{ prd.name }}</h3>
      </div>
    </div>
    <div class="card mt-4" v-if="prd">
      <div class="card-body">
        <h2 class="card-title">Ingredients</h2>
        <ul class="list-group list-group-flush">
          <li class="list-group-item" v-for="(ingredient, index) in prd.ingredients" :key="index">{{ ingredient }}</li>
        </ul>
      </div>
    </div>
    <div class="card mt-4" v-if="prd">
      <div class="card-body">
        <h2 class="card-title">Instructions</h2>
        <ul class="list-group list-group-flush">
          <li class="list-group-item" v-for="(instruction, index) in prd.instructions" :key="index">{{ instruction }}</li>
        </ul>
      </div>
    </div>
    <div v-else>
      <p>Loading...</p>
    </div>
  </div>
</template>

<script>
export default {
  name: "RecipesDetails",
  data() {
    return {
      prd: null
    };
  },
  created() {
    const id = this.$route.params.id;
    this.$store.dispatch('getRecipeById', id)
      .then(recipe => {
        this.prd = recipe;
      })
      .catch(error => {
        console.error(error);
      });
  }
};
</script>

<style scoped>
</style>
