<template>
      <v-card class="piece">
      <v-card @mouseenter="hoverOver"
            @mouseleave="deHover" 
            @mouseup="clickUp" 
            @mousedown="clickDown" 
            class="hoverBox" 
            v-bind:class="[{standard: true}, { selected : letter.selected},{hover : hovered},{hoverAndSelected : (letter.selected && hovered)}]">
                 <div class="letter">{{letter.letter}}</div>
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
    width: 100%;
    height: 10vh;
    background-color: coral;
    border: 1px solid black;
}
.hoverBox {
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
    background-color:firebrick;
    width: 50%;
    height: 50%;
}
.letter {
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
}
.standard {
    font-size: 35px;
    font-weight: bold;
}
.selected {
    background-color: green;
    width: 60%;
    height: 60%;
    font-size: 50px;
    font-weight: bolder;
}
.hover {
    width: 80%;
    height: 80%;
    background-color:olivedrab;
    font-size: 50px;
}
.hoverAndSelected {
    width: 80%;
    height: 80%;
    background-color:darkred;
    font-size: 35px;
}

</style>
