<template>
  <ion-page>
    <ion-tabs>
      <ion-router-outlet></ion-router-outlet>
      <ion-tab-bar slot="bottom">
        <ion-tab-button tab="tab1" href="/tabs/tab1">
          <ion-icon :icon="shuffle" />
          <ion-label>Random</ion-label>
        </ion-tab-button>

        <ion-tab-button tab="tab2" href="/tabs/tab2">
          <ion-icon :icon="pint" />
          <ion-label>Search by ingredient</ion-label>
        </ion-tab-button>

        <ion-tab-button tab="tab3" href="/tabs/tab3">
          <ion-icon :icon="search" />
          <ion-label>Search</ion-label>
        </ion-tab-button>
      </ion-tab-bar>
    </ion-tabs>
  </ion-page>
</template>

<script lang="ts">
import {
  IonTabBar,
  IonTabButton,
  IonTabs,
  IonLabel,
  IonIcon,
  IonPage,
  IonRouterOutlet,
} from "@ionic/vue";
import { pint, search, shuffle } from "ionicons/icons";
// import { reactive } from "vue";
// import axios from "axios";
//import { useRouter } from "vue-router";

// interface Ingredients {
//   strIngredient1: string;
// }
export default {
  name: "Tabs",
  components: {
    IonLabel,
    IonTabs,
    IonTabBar,
    IonTabButton,
    IonIcon,
    IonPage,
    IonRouterOutlet,
  },
  setup() {
    // const state = reactive({
    //   lstIngredients: [] as Ingredients[],
    //   loader: false,
    // });

    // const fetchIngredients = async () => {
    //   state.loader = true;
    //   const res: any = await axios.get(
    //     "https://www.thecocktaildb.com/api/json/v1/1/list.php?i=list"
    //   );
    //   if (res.data) {
    //     state.lstIngredients = res.data.drinks;
    //     console.log("fetchIngredients", res.data);
    //     //sort in alphabetical order
    //     state.lstIngredients.sort(function (a, b) {
    //       return a.strIngredient1.localeCompare(b.strIngredient1);
    //     });
    //   }
    //   state.loader = false;
    // };

    // const ingredientImage = (ingredient: string) => {
    //   return `https://www.thecocktaildb.com/images/ingredients/${encodeURI(
    //     ingredient
    //   )}-Small.png`;
    // };
    //fetchIngredients();

    return {
      pint,
      search,
      shuffle,
    };
  },
};
</script>
