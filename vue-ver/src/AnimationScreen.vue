<template>
    <li class="animation-screen" :style="[position]">
        <viewer
            v-bind:style="{
                gridColumnStart:1,
                gridColumnEnd:4,
                gridRowStart:1,
                gridRowEnd:2,
            }"
            v-bind:screenId="source.id"
            v-bind:filename="source.filename"
        />
        <controlBoard
            v-bind:style="{
                gridColumnStart:1,
                gridColumnEnd:4,
                gridRowStart:2,
                gridRowEnd:3,
            }"
            v-bind:source="source"
        />
    </li>
</template>


<script>
import viewer from "./AnimationScreenView"
import controlBoard from "./AnimationScreenControlBoard"

export default {
    name:"AnimationScreen",
    props:["source"], //data from AnimationContainer
    components:{
        viewer,
        controlBoard
    },
    data:{

    },
    computed:{
        position(){
            return {
                transform: 'rotate('+this.source.rotateAngle+'deg) translate('+this.source.rotateRadius+') rotate('+this.source.rotateReverseAngle+'deg)'
            }
        }
    }
}
</script>

<style lang="scss" scoped>
.animation-screen{
    position:absolute;
    -webkit-transition: all 0.4s ease-in-out;
    -moz-transition: all 0.4s ease-in-out;
    transition: all 0.4s ease-in-out;

    display:grid;
    grid-template-columns: repeat(3,1fr);
    grid-template-rows: 4fr 1fr;

    width:200px;
    height:200px;
}
</style>