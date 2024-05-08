<template>
  <div class="container">
    <h1 class="mt-5 mb-4">{{ id ? 'Edit Recipe' : 'Add Recipe' }}</h1>
    <form @submit.prevent="id ? handlePutReq() : handlePostReq()">
      <div class="mb-3">
        <label for="name" class="form-label">Recipe Name</label>
        <input type="text" id="name" v-model.trim="name" class="form-control">
      </div>
      <div class="mb-3">
        <label for="ingredients" class="form-label">Ingredients</label>
        <textarea id="ingredients" v-model.trim="ingredients" @input="formatIngredients" class="form-control" rows="5"></textarea>
      </div>
      <div class="mb-3">
        <label for="instructions" class="form-label">Instructions</label>
        <textarea id="instructions" v-model.trim="instructions" @input="formatInstructions" class="form-control" rows="5"></textarea>
      </div>
      <div class="mb-3">
        <label for="rating" class="form-label">Rating</label>
        <input type="number" id="rating" v-model.number="rating" class="form-control">
      </div>
      <div class="mb-3">
        <label for="image" class="form-label">Image URL</label>
        <input type="text" id="image" v-model.trim="image" class="form-control">
      </div>
      <button type="submit" class="btn btn-primary">Submit</button>
    </form>
  </div>
</template>

<script>
export default {
  name: "AddRecipe",
  data() {
    return {
      id: "",
      name: "",
      ingredients: "",
      instructions: "",
      rating: 0,
      image: ""
    };
  },
  created() {
    this.id = this.$route.params.id;
    if (this.id) {
      this.getPrdById();
    }
  },
  methods: {
    handlePostReq() {
      const newRecipe = {
        name: this.name,
        ingredients: this.formatArray(this.ingredients),
        instructions: this.formatArray(this.instructions),
        rating: this.rating,
        image: this.image
      };

      this.$store.dispatch('createRecipe', newRecipe)
        .then(() => {
          console.log('Recipe created successfully');
          this.$router.push("/recipes");
        })
        .catch(error => {
          console.error('Error creating recipe:', error);
        });
    },
    getPrdById() {
      this.$store.dispatch('getRecipeById', this.id)
        .then(recipe => {
          this.name = recipe.name;
          this.ingredients = recipe.ingredients.join(', ');
          this.instructions = recipe.instructions.join('\n');
          this.rating = recipe.rating;
          this.image = recipe.image;
        })
        .catch(error => {
          console.error('Error fetching recipe:', error);
        });
    },
    handlePutReq() {
      const updatedRecipe = {
        id: this.id,
        name: this.name,
        ingredients: this.formatArray(this.ingredients),
        instructions: this.formatArray(this.instructions),
        rating: this.rating,
        image: this.image
      };

      this.$store.dispatch('updateRecipe', updatedRecipe)
        .then(() => {
          console.log('Recipe updated successfully');
          this.$router.push("/recipes");
        })
        .catch(error => {
          console.error('Error updating recipe:', error);
        });
    },
    formatArray(str) {
      return str.split('\n').map(item => item.trim()).filter(Boolean);
    }
  }
};
</script>

<style scoped>
</style>
