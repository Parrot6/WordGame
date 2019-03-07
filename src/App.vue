<template>
  <v-app>
    <v-toolbar app>
      <v-toolbar-title class="headline text-uppercase"></v-toolbar-title>
      <v-spacer></v-spacer>
      <v-btn flat href="https://github.com/vuetifyjs/vuetify/releases/latest" target="_blank">
        <span class="mr-2"></span>
      </v-btn>
    </v-toolbar>
    <v-layout row class="mainContent">
        <v-content class="board" align-center="true" fluid>
                <v-flex class="scoreHeader">
                    Score: {{score}}
                </v-flex>
                <v-layout shrink grid wrap v-if="letters.length > 0">
                  <v-flex row v-for="i in Math.ceil(letters.length / 5)" :key="i">
                    <v-flex v-for="letter in letters.slice((i - 1) * 5, i * 5)" :key="letter.index">
                      <GamePiece :letter="letter" @picked="letterPicked"/>
                    </v-flex>
                  </v-flex>
                </v-layout>
                <v-layout class="currentwordandsubmit">
                  <v-flex  class="currentword">
                    {{currWordString}}
                  </v-flex>
                  <button v-on:click="submitWord">Submit</button>
                </v-layout>
        </v-content>
        <v-content class="wordlist">
          <v-flex>
            Submitted Words
          </v-flex>
          <v-flex v-for="Words in submittedWords" :key="Words">{{Words}}<br/></v-flex>
        </v-content>
    </v-layout>
    
  </v-app>
</template>

<script lang="ts">
import GamePiece from "./components/GamePiece.vue";
import Vue from "vue";
import Component from "vue-class-component";
import Letter from "./components/Letter";
import axios from 'axios';


@Component({
  components: {
    GamePiece
  }
})

export default class App extends Vue {
  name: string = "App";
  letters : Letter[] = [];
  currWord: Letter[] = [];
  currWordString: string = "";
  weightedLetters: {L : string,W:number}[] 
                         = [{L:'A', W:12},{L:'B', W:1},{L:'C', W:5},{L:'D', W:6},{L:'E', W:19},{L:'F', W:4},{L:'G', W:3},{L:'H', W:5},{L:'I', W:11},
                         {L:'J', W:1},{L:'K', W:1},{L:'L', W:5},{L:'M', W:4},{L:'N', W:11},{L:'O', W:11},{L:'P', W:4},{L:'Q', W:1},{L:'R', W:12},
                         {L:'S', W:9},{L:'T', W:13},{L:'U', W:4},{L:'V', W:1},{L:'W', W:2},{L:'X', W:1},{L:'Y', W:3},{L:'Z', W:1},];
  weight : number = 138;
  submittedWords: string[] = [];
  score : number = 0;
  letterPicked( letter: Letter ){
    console.log('got an event ' + JSON.stringify(letter));

    if(this.validChoice(letter)){
      if(!letter.selected) {
        letter.selected = !letter.selected;
        this.currWord.push(letter);
        }
      else if(this.currWord[this.currWord.length-1].index === letter.index){
      letter.selected = !letter.selected;
      this.currWord.pop();
      }
    }
    this.currentword();
    console.log('event after ' + JSON.stringify(letter));
  }
  validChoice (choice: Letter){
    if(this.currWord.length === 0) return true;
    if(this.currWord[this.currWord.length-1].index === choice.index) return true;
    if(this.currWord[this.currWord.length-1].index + 4 === choice.index) return true;
    if(this.currWord[this.currWord.length-1].index + 5 === choice.index) return true;
    if(this.currWord[this.currWord.length-1].index + 6 === choice.index) return true;
    if(this.currWord[this.currWord.length-1].index - 4 === choice.index) return true;
    if(this.currWord[this.currWord.length-1].index - 5 === choice.index) return true;
    if(this.currWord[this.currWord.length-1].index - 6 === choice.index) return true;
    if(this.currWord[this.currWord.length-1].index + 1 === choice.index) return true;
    if(this.currWord[this.currWord.length-1].index - 1 === choice.index) return true;
    return false;
  }
  currentword() {
    this.currWordString = "";
    for(let i = 0; i < this.currWord.length; i++){
      this.currWordString += this.currWord[i].letter;
    }
  }
  submitWord(){
    //if legal word
    if(this.currWordString === "") return;
    /* axios
      .get("https://googledictionaryapi.eu-gb.mybluemix.net/?define"+this.currWordString)
      .then(response => (this.info = response)).catch(error => console.log(error)) 
    console.log('query word ' + this.info)*/
    this.scoreThis(this.currWordString);
    this.submittedWords.push(this.currWordString);
    
    for(let i = 0; i < this.currWordString.length; i++){
      this.currWord[i].selected = false;
      this.currWord[i].letter = this.weightedRandomLetter();
    }
    this.currWord.length = 0;
    this.currWordString = "";
  }
  scoreThis(word: string){
    var tempscore = 0;
    if(word.length <= 3) tempscore += 3;
    else if(word.length <= 4) tempscore += 5;
    else if(word.length <= 5) tempscore += 7;
    else if(word.length <= 6) tempscore += 9;
    else if(word.length <= 7) tempscore += 12;
    else if(word.length > 7) tempscore += (word.length * 2)
    this.score += tempscore;
  }
  weightedRandomLetter(){
    var rand = Math.floor(Math.random() * this.weight);
    var total = 0;
    for(let i = 1; i < 26; i++){
      rand -= this.weightedLetters[i].W;
      if(rand <= 0) return this.weightedLetters[i].L;
      total += this.weightedLetters[i].W;
    }
    console.log(total);
    return null;
  }
  created(){
    for( let i=0; i < 25; i++){
      this.letters.push({ letter: this.weightedRandomLetter(), selected: false, index: i});
    }
  }
}
</script>
<style>
.board {
  padding: 0;
  max-width: 50vh;
}
.currentword {
  width: 100%;
  min-height: 4.5vh;
  background-color: grey;
  justify-content:space-evenly;
  font-size: 30px;
  font-weight: bolder;
  text-align: center;
}
button {
  font-weight: bold;
  font-size: 20px;
  min-width: 10vh;
  padding: 2px;
  border: 2px solid black;
  background-color: lightgreen;
}
.currentwordandsubmit {
  margin-top: 10px;
}
.scoreHeader{
  text-align: center;
  font-size: 15px;
  font-weight: bolder;
}
.wordlist {
  text-align: center;
  max-width: 15vh;
  font-size: 15px;
  font-weight: bolder;
}
.mainContent {
  margin-left: 35vw;
  margin-top: 10vh;
  width: 100vh;
}
</style>
