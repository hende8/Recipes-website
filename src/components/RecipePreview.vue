<template>
  <div class="card" style="width: 20rem;  ">
    <router-link
      :to="{ name: 'recipe', params: { recipeId: recipe.recipeID } }"
    >
      <div class="recipe-body">
        <img  id="card1" :src="recipe.imageURL" class="card-img-top" style="max-width:318px; min-width:318px max-height:230px; min-height:230px"/>
      </div>
    </router-link>
    <div class="card-body" style="max-height:230px; min-height:230px">
      <b-row>
        <b-col>
          <b-icon
            v-if="recipe.isWatch && this.$root.store.username && !isInternal"
            icon="eye-fill"
            font-scale="1.3"
          ></b-icon>
        </b-col>
        <br />
        <div class="divider" />
        <b-col>
          <favoriteButton
            v-if="this.$root.store.username && !isInternal"
            :isFavorite="recipe.isFavorite"
            :recipeID="recipe.recipeID"
          ></favoriteButton>
        </b-col>
      </b-row>
        <div class="recipe-footer">
          <h6 :title="recipe.name" class="card-title">{{ recipe.name }}</h6>
          
          <ul class="card-text">
            <b-row>
              <b-icon
                icon="clock"
                variant="dark"
                font-scale="1.3"
                style="margin-right:15px"
              ></b-icon>
              {{ recipe.cookingDuration }}
              minutes</b-row
            >
            <b-row>
              <b-icon
                icon="hand-thumbs-up"
                variant="dark"
                font-scale="1.4"
                style="margin-right:15px"
              ></b-icon>
              {{ recipe.likes }}
              likes</b-row
            >
            <b-row v-if="recipe.isVegeterian">
              <img
                src="https://img.icons8.com/color/25/000000/vegetarian-mark.png"
                style="margin-right:13px"
              />
              Vegetarian
            </b-row>
            <b-row v-if="recipe.isVegan">
              <img
                src="https://img.icons8.com/color/25/000000/vegan-symbol.png"
                style="margin-right:13px"
              />
              Vegan
            </b-row>
            <b-row v-if="recipe.isGluten">
              <img
                src="https://img.icons8.com/color/25/000000/no-gluten.png"
                style="margin-right:13px"
              />
              Gluten free
            </b-row>
          </ul>
        </div>
    </div>
  </div>
</template>

<script>
import favoriteButton from "./favoriteButton.vue";
export default {
  components: {
    favoriteButton,
  },
  props: {
    recipe: {
      type: Object,
      required: true,
    },
    isInternal: {
      type: Boolean,
      default: false,
    },
  },
};
</script>

<style scoped>
.divider {
  display: inline-block;
  margin-right: 10em;
  
}
.card {
  margin: 0 auto; /* Added */
  float: none; /* Added */
  margin-bottom: 10px; /* Added */
 
}
#card1:hover {
   transform: scale(1.02);
}
/* .recipe-preview {
  display: inline-block;
  width: 90%;
  height: 100%;
  position: relative;
  margin: 10px 10px;
}
.recipe-preview > .recipe-body {
  width: 100%;
  height: 100;
  position: relative;
}

.recipe-preview .recipe-body .recipe-image {
  margin-left: auto;
  margin-right: auto;
  margin-top: auto;
  margin-bottom: auto;
  display: block;
  width: 70%;
  height: auto;
  -webkit-background-size: cover;
  -moz-background-size: cover;
  background-size: cover;
}

.recipe-preview .recipe-footer {
  margin-left: auto;
  margin-right: auto;
  margin-top: auto;
  margin-bottom: auto;
  width: 70%;
  height: 50%;
  overflow: hidden;
}

.recipe-preview .recipe-footer .recipe-title {
  padding: 10px 10px;
  width: 100%;
  font-size: 12pt;
  text-align: left;
  white-space: nowrap;
  overflow: hidden;
  -o-text-overflow: ellipsis;
  text-overflow: ellipsis;
}

.recipe-preview .recipe-footer ul.recipe-overview {
  padding: 5px 10px;
  width: 100%;
  display: -webkit-box;
  display: -moz-box;
  display: -webkit-flex;
  display: -ms-flexbox;
  display: flex;
  -webkit-box-flex: 1;
  -moz-box-flex: 1;
  -o-box-flex: 1;
  box-flex: 1;
  -webkit-flex: 1 auto;
  -ms-flex: 1 auto;
  flex: 1 auto;
  table-layout: fixed;
  margin-bottom: 0px;
}

.recipe-preview .recipe-footer ul.recipe-overview li {
  -webkit-box-flex: 1;
  -moz-box-flex: 1;
  -o-box-flex: 1;
  -ms-box-flex: 1;
  box-flex: 1;
  -webkit-flex-grow: 1;
  flex-grow: 1;
  width: 90px;
  display: table-cell;
  text-align: center;
}

.recipe-info-icon {
  width: 50%;
} */
</style>
