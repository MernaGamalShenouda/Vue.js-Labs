<template>
  <div>
    <h1>Recipes Page</h1>
    <table class="table">
      <thead>
        <tr>
          <th scope="col">ID</th>
          <th scope="col">Name</th>
          <th scope="col">Rating</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="recipe in recipes" :key="recipe.id">
          <td>{{ recipe.id }}</td>
          <td>{{ recipe.name }}</td>
          <td>{{ recipe.rating }}</td>
          <td><button class="btn btn-dark" @click="handleDelete(recipe.id)">del</button></td>
          <td><router-link class="btn btn-dark" :to="`/recipes/${recipe.id}`">see more..</router-link></td>
          <td><router-link class="btn btn-dark" :to="`/add/${recipe.id}`">Edit..</router-link></td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: "RecipesPage",
  computed: {
    recipes() {
      return this.$store.getters.getRecipes;
    }
  },
  methods: {
    handleDelete(id) {
      this.$store.dispatch('deleteRecipe', id)
        .then(() => {
          console.log('Recipe deleted successfully');
        })
        .catch(error => {
          console.error('Error deleting recipe:', error);
        });
    }
  },
  created() {
    this.$store.dispatch('getAllRecipes')
      .then(() => {
        console.log('Recipes loaded successfully');
      })
      .catch(error => {
        console.error('Error loading recipes:', error);
      });
  }
};
</script>

<style scoped>
</style>
