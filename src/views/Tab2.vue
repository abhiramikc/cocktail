<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-title>Search by incredient</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content v-if="state.loader">
      <div class="loading-center">
        <ion-spinner name="lines" color="primary"></ion-spinner>
      </div>
    </ion-content>
    <ion-content :fullscreen="true" v-else>
      <ion-list>
        <ion-item
          v-for="ingredient in state.lstIngredients"
          :key="ingredient.strIngredient1"
          @click="
            () => $router.push(`/drinks-by-ingredient/${ingredient.strIngredient1}`)
          "
        >
          <ion-avatar slot="start">
            <img :src="ingredientImage(ingredient.strIngredient1)" />
          </ion-avatar>
          <ion-label>
            <h2>{{ ingredient.strIngredient1 }}</h2>
          </ion-label>
        </ion-item>
      </ion-list>
    </ion-content>
  </ion-page>
</template>

<script lang="ts">
import {
  IonHeader,
  IonLabel,
  IonPage,
  IonList,
  IonItem,
  IonContent,
  IonSpinner,
  IonTitle,
  IonToolbar,
  IonAvatar,
} from "@ionic/vue";
import { pint, square, shuffle } from "ionicons/icons";
import { reactive } from "vue";
import axios from "axios";
import { useRouter } from "vue-router";
import Iingredients from "@/interfaces/IIngredient";

export default {
  name: "Tabs",
  components: {
    IonPage,
    IonAvatar,
    IonHeader,
    IonList,
    IonItem,
    IonContent,
    IonSpinner,
    IonLabel,
    IonTitle,
    IonToolbar,
  },
  setup() {
    const state = reactive({
      lstIngredients: [] as Iingredients[],
      loader: false,
    });

    const fetchIngredients = async () => {
      state.loader = true;
      const res: any = await axios.get(
        "https://www.thecocktaildb.com/api/json/v1/1/list.php?i=list"
      );
      if (res.data) {
        state.lstIngredients = res.data.drinks;
        console.log("fetchIngredients", res.data);
        //sort in alphabetical order
        state.lstIngredients.sort(function (a, b) {
          return a.strIngredient1.localeCompare(b.strIngredient1);
        });
      }
      state.loader = false;
    };

    const ingredientImage = (ingredient: string) => {
      return `https://www.thecocktaildb.com/images/ingredients/${ingredient}-Small.png`;
      //return `https://www.thecocktaildb.com/images/ingredients/7-Up-Small.png`;
    };
    fetchIngredients();

    return {
      pint,
      square,
      shuffle,
      state,
      ingredientImage,
      useRouter,
    };
  },
};
</script>
<style>
.loading-center {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 90vh;
}
ion-spinner {
  transform: scale(1.5);
}
</style>
