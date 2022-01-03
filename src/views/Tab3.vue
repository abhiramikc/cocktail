<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-title>Search</ion-title>
      </ion-toolbar>
      <ion-toolbar>
        <ion-searchbar
          debounce="500"
          :onIonChange="(e) => fetchSearchResult(e.detail.value)"
        ></ion-searchbar>
      </ion-toolbar>
    </ion-header>
    <ion-content :fullscreen="true" v-if="state.loader">
      <div class="center">
        <ion-spinner name="lines" color="primary"></ion-spinner>
      </div>
    </ion-content>
    <ion-content :fullscreen="true" v-else>
      <div class="center" v-if="state.searchResults && state.searchResults.length === 0">
        <ion-label>No Results</ion-label>. Please search Above
      </div>
      <div>
        <DrinkCard
          v-for="drink in state.searchResults"
          :key="drink.idDrink"
          :drink="drink"
        ></DrinkCard>
      </div>
    </ion-content>
  </ion-page>
</template>

<script lang="ts">
import {
  IonPage,
  IonSpinner,
  IonHeader,
  IonToolbar,
  IonTitle,
  IonContent,
  IonLabel,
  IonSearchbar,
} from "@ionic/vue";
import { onMounted, reactive } from "vue";
import axios from "axios";
import iDrinkDetails from "@/interfaces/IDrinkDetails";
import DrinkCard from "../components/DrinkCard.vue";

export default {
  name: "Tab3",
  components: {
    IonSpinner,
    IonHeader,
    IonToolbar,
    IonTitle,
    IonContent,
    IonPage,
    IonSearchbar,
    IonLabel,
    DrinkCard,
  },
  setup() {
    const state = reactive({
      searchResults: [] as iDrinkDetails[],
      loader: false,
    });
    onMounted(() => {
      console.log("Component is mounted!");
    });
    const fetchSearchResult = async (searchTerm: string) => {
      state.loader = true;
      const result: any = await axios.get(
        `https://www.thecocktaildb.com/api/json/v1/1/search.php?s=${searchTerm}`
      );

      if (result.data) {
        state.searchResults = result.data.drinks;
        console.log("Serch result", result.data.drinks);
      }
      state.loader = false;
    };

    return {
      state,
      fetchSearchResult,
    };
  },
};
</script>
<style>
.center {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 90vh;
}
ion-spinner {
  transform: scale(1.5);
}
</style>
