<template>
  <v-app>
    <v-toolbar app>
      <v-toolbar-title class="headline text-uppercase"></v-toolbar-title>
      <v-spacer></v-spacer>
      <v-btn flat href="https://github.com/vuetifyjs/vuetify/releases/latest" target="_blank">
        <span class="mr-2"></span>
      </v-btn>
    </v-toolbar>

    <v-content class="mainContent">
            <v-layout row wrap v-if="letters.length > 0" shrink>
              <v-flex style="width: 20%;" v-for="letter in letters" :key="letter.index" shrink>
                  <GamePiece :letter="letter" @picked="letterPicked"/>
              </v-flex>
            </v-layout>
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
    GamePiece
  }
})
export default class App extends Vue {
  name: string = "App";
  letters : {letter: string, selected: boolean, index: number}[] = [];

  availableLetters: string = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ';
  letterPicked( letter: any ){
    console.log('got an event ' + JSON.stringify(letter));
    this.letters[letter.index].selected = !this.letters[letter.index].selected;
  }
  created(){
    for( let i=0; i < 25; i++){
      this.letters.push({ letter: this.availableLetters[Math.floor(Math.random() * 26)], selected: false, index: i});
    }
  }
}
</script>
<style>
.mainContent {
  margin-right:30%;
  margin-left: 30%;
  margin-top: 100px;
}
</style>
