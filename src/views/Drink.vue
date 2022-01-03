<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-buttons slot="start">
          <ion-back-button> </ion-back-button>
        </ion-buttons>
        <ion-title>{{ state.drink.strDrink }}</ion-title>
      </ion-toolbar>
    </ion-header>

    <ion-content v-if="state.loader">
      <div class="loading-center">
        <ion-spinner name="lines" color="primary"></ion-spinner>
      </div>
    </ion-content>
    <ion-content :fullscreen="true" v-else>
      <DrinkCard :drink="state.drink" />
    </ion-content>
  </ion-page>
</template>
<script lang="ts">
import { defineComponent, onMounted } from "vue";
import { reactive } from "vue";
import { useRoute } from "vue-router";
import axios from "axios";
import DrinkCard from "../components/DrinkCard.vue";
import iDrinkDetails from "@/interfaces/IDrinkDetails";
import {
  IonPage,
  IonSpinner,
  IonHeader,
  IonToolbar,
  IonTitle,
  IonContent,
  IonButtons,
  IonBackButton,
} from "@ionic/vue";

export default defineComponent({
  name: "Drink",
  components: {
    IonPage,
    IonSpinner,
    IonHeader,
    DrinkCard,
    IonToolbar,
    IonTitle,
    IonContent,
    IonButtons,
    IonBackButton,
  },
  setup() {
    const route = useRoute();
    const drinkId = route.params.id as string;
    console.log("drinkId", drinkId);

    const state = reactive({
      drink: {} as iDrinkDetails,
      loading: true,
    });
    onMounted(() => {
      console.log("Component is mounted!");
    });
    const fetchDrinkById = async (drinkId: string) => {
      state.loading = true;
      const result: any = await axios.get(
        `https://www.thecocktaildb.com/api/json/v1/1/lookup.php?i=${drinkId}`
      );
      console.log("Drink fetch by id", result.data);
      if (result.data) {
        state.drink = result.data.drinks[0];
        console.log("state.drink", state.drink);
      }
      state.loading = false;
    };
    fetchDrinkById(drinkId);
    return {
      state,
    };
  },
});
</script>
