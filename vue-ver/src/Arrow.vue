<template>
    <connection  :from="'#screen-'+from" :to="'#screen-'+to" color="red" tail :width="3">
        <div class="line" v-on:click="disconnect" v-on:mouseover="showTransitionCondition"></div>
    </connection>
</template>

<script>
import * as Arrow from './domarrow'

export default {
    name:"Arrow",
    props:{
        from:{
            type:Number,
            required:true
        },
        to:{
            type:Number,
            required:true
        }
    },
    methods:{
        disconnect(){
            this.$emit('disconnect',[this.from,this.to])
        },
        showTransitionCondition(){
            this.$emit('showTransitionCondition',[this.from,this.to] )
        }
    },
    mounted(){

    },
    updated(){

    }
}
</script>

<style lang="scss">
connection{
    .line{
        cursor:pointer;
        position:absolute;
        height:20px;
        border-color: purple;
        background-color: black;
        .arrow{
            top: -5px;
            height: 0px;
            width: 0px;
            position: absolute;
            border-bottom: 6px solid transparent;
            border-top: 6px solid transparent;
            background-clip: border-box;
        }
        .arrow-fw{
            border-right: 12px solid black;
        }
        .arrow-bw{
            left: 100%;
            border-left: 12px solid black;
            transform: translateX(-12px);
        }
        .text{
            position: absolute;
            top: 100%;
            left: 50%;
            transform: translate(-50%, -100%);
        }
    }
}
</style>