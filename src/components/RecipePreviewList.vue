<template>
  <div > 
    <br>
    <h1  style=" text-align:center;  color:whitesmoke;">
      {{ title }}:
      <slot></slot>
    </h1>
    <br>
    <div v-if="this.recipes== null"  style=" text-align:center;  color:whitesmoke;">
      <strong style="color:whitesmoke; ">Loading...</strong>
    </div>
    <h3 v-if="this.recipes==false"  style=" text-align:center;  color:whitesmoke;"> No recipes watched</h3>
    <div v-else >
    <b-row  v-for="r in this.recipes" :key="r.id">
      <RecipePreview class="p" :recipe="r" :key="render"></RecipePreview>
    </b-row>
    </div>
    

  </div>
</template>

<script>
import RecipePreview from "./RecipePreview.vue";
export default {
  name: "RecipePreviewList",
  components: {
    RecipePreview,
  },
  props: {
    title: {
      type: String,
      required: true,
    },
    action: {
      type: String,
      required: true,
    },
    rerender: {
      type: Number,
    },
  },
  data() {
    return {
      recipes: null,
      buttonAction: true,
      render: 0,
    };
  },
  async created() {
    if (this.action == "random") {
      this.updateRandomRecipes();
    } else {
      this.updateLastView();
      this.buttonAction = false;
    }
  },
  methods: {
    async updateRandomRecipes() {
      try {
        const response = await this.axios.get(
          "http://localhost:3000/recipes/randomRecipes"
        );
        const recipes = response.data;
        // this.recipes = [];
        // this.recipes.push(...recipes);
        if (this.$root.store.username) {
          await this.getUserInformation(recipes);
        } else {
          recipes.map((x) => {
            x.isFavorite = false;
          });
          this.recipes = [];
          this.recipes.push(...recipes);
        }
      } catch (error) {
      }
    },

    async updateLastView() {
      try {
        let recipes = [];
        if (!this.$store.lastWatch) {
          const response = await this.axios.get(
            // "https://assignment3-2-shiran-hen.herokuapp.com/user/myWatch"
            "http://localhost:3000/user/myWatch"
          );

          recipes = response.data;
          if (recipes.length !== 0) {
            this.$store.lastWatch = recipes;
          }
        } else {
          recipes = this.$store.lastWatch;
        }

        if (recipes.length!==0) {
          this.getUserInformation(recipes);
        }else{
          this.recipes=false;
        }


      } catch (error) {
      }
    },

    async getUserInformation(recipes) {
      try{
      let recipeIDArray = [];
      recipes.map((x) => recipeIDArray.push(x.recipeID));

      let info = await this.axios.get(
        "http://localhost:3000/user/search/" + JSON.stringify(recipeIDArray)
      );
      let newRecipes = [];
      info.data.forEach((element) => {
        let recipe = recipes.find((el) => el.recipeID == element.recipeID);
        if (this.action == "random") {
          recipe.isWatch = element.isWatch;
        }
        recipe.isFavorite = element.isFavorite;
        newRecipes.push(recipe);
      });
      this.recipes = [];
      this.recipes.push(...newRecipes);
      }catch(err){
        
      }

    },
  },
  watch: {
    rerender: async function() {
      await this.getUserInformation(this.recipes);
      this.render += 1;
    },
  },
};
</script>

<style lang="scss" scoped>
// .p{
//   background-image: url("../assets/bgbody.jpg");
// }
// .container {
//   min-height: 400px;
// }
// .split {
//   height: 50%;
//   width: 50%;
//   // position: fixed;
//   z-index: 1;
//   // // top: 0;
//   overflow-x: hidden;
//   padding-top: 20px;
// }

// .left {
//   left: 0;
// }
</style>
