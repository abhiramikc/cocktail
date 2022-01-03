<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-buttons slot="start">
          <ion-back-button> </ion-back-button>
        </ion-buttons>
        <ion-title>{{ ingredient }}</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content :fullscreen="true" v-if="state.loader">
      <div class="loading-center">
        <ion-spinner name="lines"></ion-spinner>
      </div>
    </ion-content>
    <ion-content :fullscreen="true" v-else>
      <ion-list>
        <ion-item
          v-for="drink in state.lstDrinks"
          :key="drink.idDrink"
          @click="() => $router.push(`/drink/${drink.idDrink}`)"
        >
          <ion-avatar slot="start">
            <img :src="drink.strDrinkThumb" />
          </ion-avatar>
          <ion-label>
            <h2>{{ drink.strDrink }}</h2>
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
  IonButtons,
  IonBackButton,
} from "@ionic/vue";
import { reactive } from "vue";
import axios from "axios";
import { useRouter, useRoute } from "vue-router";
import IDrink from "../interfaces/IDrink";
export default {
  name: "DrinksByIngredients",
  components: {
    IonPage,
    IonButtons,
    IonBackButton,

    IonAvatar,
    IonHeader,
    IonList,
    IonItem,
    IonContent,
    IonLabel,
    IonTitle,
    IonToolbar,
    IonSpinner,
  },
  setup() {
    const router = useRouter();
    const route = useRoute();
    const ingredient = route.params.ingredient as string;
    const state = reactive({
      lstDrinks: [] as IDrink[],
      loader: false,
    });

    const fetchDrinkByIngredients = async (ingredient: string) => {
      state.loader = true;

      const res: any = await axios.get(
        `https://www.thecocktaildb.com/api/json/v1/1/filter.php?i=${ingredient}`
      );
      if (res.data) {
        state.lstDrinks = res.data.drinks;
        console.log("fetchIngredients", res.data);
        //sort in alphabetical order
      }
      state.loader = false;
    };

    fetchDrinkByIngredients(ingredient);

    return {
      ingredient,
      state,
      useRouter,
      router,
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
