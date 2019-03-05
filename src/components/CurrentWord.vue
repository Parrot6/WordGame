<template>
      <v-card class="word">
          {{ThisWord}}
      </v-card>
</template>

<script lang="ts">
import Vue from "vue";
import Component from "vue-class-component";

export default {
    name: 'CurrentWord',
    data: function(){
        return{
        ThisWord: 'tedadadawdawdawdawdasdasdasdst'
        }
    },
    methods: {
        pushLetter: function(letter: CharacterData) {
            this.ThisWord = this.ThisWord + letter
        },
        removeLetter: function(letter: CharacterData) {
            this.ThisWord = this.ThisWord.slice(1,this.ThisWord.length - 1)
        }
    },
    props: {
      word: {
      type: String,
      required: false,
      default: 'default'
      }
    },
    created: function() {
        this.ThisWord = this.word
    }

}
</script>
<style scoped>
.word {
    color: black;
    text-align: center;
    vertical-align: middle;
    width: 115%;
    height: 50px;
    Font-size: 30;
    background-color: gray;
    border: 2px solid black;
}
</style>
