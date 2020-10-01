<template>
    <div class='animation-container'>
        <div class='storyboard-control-board'>
            <button v-on:click="pushScreen">+</button>
            <button v-on:click="popScreen">-</button>
        </div>
        <div class="storyboard">
            <ul :style="[positionAndSize]">
                <screen
                    v-for="film in animations"
                    v-bind:key="film.id"
                    v-bind:source="film"
                >
                </screen>
            </ul>
        </div>
    </div>
</template>



<script>
import screen from './AnimationScreen'

export default {
    name:'AnimationContainer',
    components:{
        screen
    },
    data(){
        var animations = []
        return{
            animations:animations
        }
    },
    computed:{
        positionAndSize:function(){
            const distance = this.calculateRadius(this.animations.length)
            const height = this.calculateRadius(this.animations.length, 2)
            return{
                marginTop: distance,
                marginLeft: distance,
                width:height,
                height:height,
            }
        }
    },
    methods:{
        calculateRadius:function(numOfItem, factor=1){
            return factor*Math.sqrt(numOfItem)*8+"em"
        },
        calculateAngle:function(idx, totalNumOfItem){
            const startAngle = -90
            const type = 1 // circle type -> 1: whole, 0.5 :half, 0.25 : quarter
            const angle = 360*type/totalNumOfItem
            return angle*idx+startAngle
        },

        pushScreen:function(){
            //add screen
            this.animations.push({
                id:this.animations.length,
                animationName:"default not set",
                rotateRadius: this.calculateRadius(this.animations.length+1), // becuase item will be pushed
                rotateAngle: this.calculateAngle(this.animations.length-1, this.animations.length),
                rotateReverseAngle: -1*this.calculateAngle(this.animations.length-1, this.animations.length)
            })

            //re calculate tpositions of screen items
            for(let i=0; i<this.animations.length; i++){
                const a = this.calculateAngle(i,this.animations.length)
                this.animations[i].rotateRadius = this.calculateRadius(this.animations.length)
                this.animations[i].rotateAngle = a
                this.animations[i].rotateReverseAngle = -1*a
            }
            return this.animations.length
        },
        popScreen:function(){
            const val = this.animations.pop()
            //re calculate positions of screen items
            for(let i=0; i<this.animations.length; i++){
                const a = this.calculateAngle(i,this.animations.length)
                this.animations[i].rotateRadius = this.calculateRadius(this.animations.length)
                this.animations[i].rotateAngle = a
                this.animations[i].rotateReverseAngle = -1*a
            }
            return val
        }
    }


}
</script>

<style lang="scss" scoped>
.animation-container{
    width:100%;
    height:100%;
    ul{
        list-style-type: none;
        margin:0;
        -webkit-transition: all 0.4s ease-in-out;
        -moz-transition: all 0.4s ease-in-out;
        transition: all 0.4s ease-in-out;
    }
    display: grid;
    grid-template-columns: repeat(3,1fr);
    grid-template-rows: 1fr 4fr;

    .storyboard-control-board{
        grid-column-start: 1;
        grid-column-end: 4;
        grid-row-start: 1;
        grid-row-end:2;
    }
    .storyboard{
        grid-column-start: 1;
        grid-column-end: 4;
        grid-row-start: 2;
        grid-row-end: 3;
    }
}
</style>