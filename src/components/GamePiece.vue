<template>
      <v-card class="piece">
      <v-card @mouseenter="hoverOver" @mouseleave="deHover" @mouseup="clickUp" @mousedown="clickDown" class="hoverBox" v-bind:class="[{ selected : isSelected},{hover : hovered},{hoverAndSelected : (isSelected && hovered)}]">{{letter}}</v-card>
      </v-card>
</template>

<script lang="ts">
import Vue from "vue";
import Component from "vue-class-component";
import CurrentWord from '@/components/CurrentWord.vue';

export default {
    name: 'Piece',
    props : {
        CurrentWord
    },
    data: function() {
        return{
            hoverclickcolor: 'red',
            hovered: false,
            isSelected: false,
            isClickedDown: false,
            letter: '',
            letters: 'ABCDEFGHIJKLMNOPQRSTUVWXYZ',
        }
    },
    methods: {
        clickDown: function () {
         this.isClickedDown = true
         /* if(this.isSelected) CurrentWord.removeLetter() */
         this.isSelected = !this.isSelected
         this.deHover()
        },
        clickUp: function () {
         this.isClickedDown = false
        },
        hoverOver: function () {
        this.hovered = true
        }
        ,
        deHover: function () {
         this.hovered = false
        }
    },
    created: function() {
    this.letter = this.letters[Math.floor(Math.random() * 26)]
  }

}
</script>
<style scoped>
.piece {
    display: flex;
    width: 75px;
    height: 75px;
    background-color: coral;
    border: 1px solid black;
    margin: 1px;
    justify-content: center;
    align-items: center; 
}
.hoverBox {
    display: flex;
    background-color:firebrick;
    width: 50%;
    height: 50%;
    justify-content: center;
    align-items: center; 
}
.selected {
    background-color: green;
    width: 60%;
    height: 60%;
    font-size: 20px;
    font-weight: bolder;
}
.hover {
    width: 80%;
    height: 80%;
    background-color:olivedrab;
    font-size: 17px;
}
.hoverAndSelected {
    width: 80%;
    height: 80%;
    background-color:darkred;
    font-size: 17px;
}

</style>
