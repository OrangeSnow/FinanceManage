<template>
    <div class="container-view">
        <head-title :title="'分析：'"></head-title>
        <div class="chart-wrap">
            <scroller lock-x
                      height="-118"
                      :scrollbarY="true"
                      @on-scroll="onScroll"
                      ref="chartScrollEvent">
                <div class="chart-con">                            
                    <div class="maid">哈哈哈哈哈哈哈</div>
                    <div class="maid">哈哈哈哈哈哈哈</div>
                    <div class="maid">哈哈哈哈哈哈哈</div>
                </div>
            </scroller>
        </div>
    </div>
</template>
<script>
    import types from '../../store/mutation-types'
    import headTitle from '../../components/head-title.vue'
    import Util from '../../assets/lib/Util'
    import Tool from '../../assets/lib/Tool'
    import { Scroller } from 'vux'
    export default {
        name: 'chart',
        data () {
            return {
                consumption_chart_arr: [0,0,0,0,0,0,0,0],
                earn_chart_arr: [0,0,0]
            }
        },
        components: {
            Scroller,
            headTitle
        },
        created () {
            this.$store.commit(types.SET_NAV_INDEX,'4');
            this.fetchBillData();
        },
        methods: {
            onScroll (pos) {
                this.scrollTop = pos.top;
            }
            
        }
    }
</script>
<style lang="scss">
    @import "../../assets/scss/define";
    .chart-wrap{
        @extend %oya;
        @extend %pa;
        @extend %w100;
        @extend %ios;
        @extend %b0;
        top: 64px;
    }
    .chart-title{
        @extend %f14;
        @extend %fwn;
        color: #58B7FF;
    }
    .chart-item{
        padding: 10px;
    }
    .maid{
    height:500px;
    width:500px;
    background-color:pink;
    }
</style>
