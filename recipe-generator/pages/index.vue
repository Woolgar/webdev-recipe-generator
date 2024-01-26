<template>
  <div class="container m-auto flex">
    <div class="md:flex md:flex-row max-w-7xl">
      <template v-for="meal in meals">
        <NuxtImg
          class="w-full object-cover rounded-l-xl md:h-auto"
          :src="meal.image"
          :alt="meal.name"
        />
        <div class="px-5 bg-white md:p-5 rounded-r-xl">
          <h1 class="text-2xl text-center md:text-left font-medium">
            {{ meal.name }}
          </h1>
          <ul class="list-disc p-5 md:flex md:flex-wrap">
            <template v-for="item in meal.ingredients">
              <li class="box-border">
                {{ item.measure }} {{ item.ingredient }}
              </li>
            </template>
          </ul>
          <h2 class="text-2xl font-medium mb-5">Directions</h2>
          <p>{{ meal.instructions }}</p>
        </div>
      </template>
    </div>
  </div>
</template>

<script setup lang="ts">
const { data: meals } = await useFetch(
  "https://www.themealdb.com/api/json/v1/1/random.php",
  {
    transform: (response: any) =>
      response.meals.map((meal: any) => {
        const ingredients = Object.keys(meal)
          .filter((key) => key.includes("strIngredient"))
          .map((key) => meal[key])
          .filter((ingredient) => ingredient !== "");

        const measures = Object.keys(meal)
          .filter((key) => key.includes("strMeasure"))
          .map((key) => meal[key])
          .filter((measure) => measure !== "");

        const ingredientsWithMeasures = ingredients.map(
          (ingredient, index) => ({
            ingredient,
            measure: measures[index] || "",
          })
        );

        return {
          id: meal.idMeal,
          name: meal.strMeal,
          category: meal.strCategory,
          area: meal.strArea,
          instructions: meal.strInstructions,
          image: meal.strMealThumb,
          tags: meal.strTags,
          youtube: meal.strYoutube,
          ingredients: ingredientsWithMeasures,
        };
      }),
  }
);
useHead({
  title: "Recipe Generator",
  meta: [
    {
      content: "A recipe generator built with Nuxt.js",
    },
  ],
});
</script>

<style lang="scss"></style>
