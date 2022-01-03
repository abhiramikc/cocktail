<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-title>Random</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content :fullscreen="true" v-if="state.loading">
      <div class="loading-center">
        <ion-spinner color="primary"></ion-spinner>
      </div>
    </ion-content>
    <ion-content :fullscreen="true" v-else>
      <ion-refresher slot="fixed" @ionRefresh="doRefresh">
        <ion-refresher-content></ion-refresher-content>
      </ion-refresher>
      <DrinkCard :drink="state.randomCocktail"></DrinkCard>
    </ion-content>
  </ion-page>
</template>

<script lang="ts">
import DrinkCard from "../components/DrinkCard.vue";
import {
  IonPage,
  IonSpinner,
  IonRefresher,
  IonRefresherContent,
  IonHeader,
  IonToolbar,
  IonTitle,
  IonContent,
} from "@ionic/vue";
import { reactive } from "vue";
import axios from "axios";
import iDrinkDetails from "@/interfaces/IDrinkDetails";
export default {
  name: "Tab1",
  components: {
    DrinkCard,
    IonSpinner,
    IonRefresher,
    IonRefresherContent,
    IonHeader,
    IonToolbar,
    IonTitle,
    IonContent,
    IonPage,
  },
  setup() {
    const state = reactive({
      randomCocktail: {} as iDrinkDetails,
      loading: false,
    });

    const fetchRandomCocktail = async (disploderPage: boolean) => {
      if (disploderPage) {
        state.loading = true;
      }
      const res: any = await axios.get(
        "https://www.thecocktaildb.com/api/json/v1/1/random.php"
      );
      if (res.data) {
        state.randomCocktail = res.data.drinks[0];
      }

      state.loading = false;
    };
    const doRefresh = (event: CustomEvent) => {
      fetchRandomCocktail(false);

      // eslint-disable-next-line @typescript-eslint/ban-ts-ignore
      //@ts-ignore
      event.target.complete();
    };
    fetchRandomCocktail(true);

    return { state, fetchRandomCocktail, doRefresh };
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
