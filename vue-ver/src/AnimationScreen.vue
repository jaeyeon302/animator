<template>
    <li class="animation-screen" :style="[position]">
        <div class="viewer">
            {{source.id}}
        </div>
        <div class="control-board">
            "hi"
        </div>
    </li>
</template>


<script>
export default {
    name:"AnimationScreen",
    props:["source"], //data from AnimationContainer
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
    
    border-radius: 10px;
    width:200px;
    height:200px;
    .viewer{
        background-color:red;
        grid-column-start: 1;
        grid-column-end:4;
        grid-row-start: 1;
        grid-row-end:2;
        border-radius: 10px 10px 0px 0px;
    }
    .control-board{
        background-color: blue;
        grid-column-start: 1;
        grid-column-end: 4;
        grid-row-start:2;
        grid-row-end:3;
        border-radius:0px 0px 10px 10px;
    }
}
</style>