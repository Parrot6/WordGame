<template>
  <v-app>
    <v-toolbar app>
      <v-toolbar-title class="headline text-uppercase"></v-toolbar-title>
      <v-spacer></v-spacer>
      <v-btn flat href="https://github.com/vuetifyjs/vuetify/releases/latest" target="_blank">
        <span class="mr-2">Latest Release</span>
      </v-btn>
    </v-toolbar>

    <v-content class="mainContent">
      <v-container justify-center>
            <v-layout row v-for="n in 5" :key="n">
              <v-flex
                v-for="n in 5"
                :key="n"
                shrink
              >
                  <GamePiece/>
              </v-flex>
            </v-layout>
            <CurrentWord word="sexytime"/>
      </v-container>
    </v-content>
  </v-app>
</template>

<script lang="ts">
import GamePiece from "./components/GamePiece.vue";
import Vue from "vue";
import Component from "vue-class-component";
import CurrentWord from "./components/CurrentWord.vue";

@Component({
  components: {
    GamePiece,
    CurrentWord
  }
})
export default class App extends Vue {
  name: string = "App";
}
</script>
<style>
.mainContent {
  margin-right:40%;
  margin-left: 40%;
  margin-top: 100px;
}
</style>
