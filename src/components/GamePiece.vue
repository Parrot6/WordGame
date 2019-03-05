<template>
      <v-card class="piece">
      <v-card @mouseenter="hoverOver"
            @mouseleave="deHover" 
            @mouseup="clickUp" 
            @mousedown="clickDown" 
            class="hoverBox" 
            v-bind:class="[{ selected : letter.selected},{hover : hovered},{hoverAndSelected : (letter.selected && hovered)}]">
            {{letter.letter}}
        </v-card>
      </v-card>
</template>

<script lang="ts">
import Vue from "vue";
import Component from "vue-class-component";
import { Prop } from 'vue-property-decorator';

@Component({})
export default class GamePiece extends Vue {
    @Prop(Object) letter: object;

    hoverclickcolor = 'red';
    hovered= false;
    isClickedDown= false;
        
    clickDown () {
        this.isClickedDown = true
        /* if(this.isSelected) CurrentWord.removeLetter() */
        this.deHover()
        this.$emit('picked', this.letter);
    }
    clickUp () {
        this.isClickedDown = false
    }
    hoverOver () {
    this.hovered = true
    }
    
    deHover () {
        this.hovered = false
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
