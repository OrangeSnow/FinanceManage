<template>
    <div class="container-view login-wrap">
        <head-title :title="'修改密码：'"></head-title>
        <div class="speechJump2"><a href="#/maid">聊天室</a></div>

        <h1>Speech synthesiser</h1>
        <p>Enter some text in the input below and press return  or the "play" button to hear it. change voices using the dropdown menu.</p>
        <form>
            <input type="text" class="txt">
            <!-- 频率调节 -->
            <div>
                <label for="rate">Rate</label><input type="range" min="0.5" max="2" value="1" step="0.1" id="rate">
                <div class="rate-value">1</div>
                <div class="clearfix"></div>
            </div>
            <!-- 音调高低调节 -->
            <div>
                <label for="pitch">Pitch</label><input type="range" min="0" max="2" value="1" step="0.1" id="pitch">
                <div class="pitch-value">1</div>
                <div class="clearfix"></div>
            </div>
            <!-- 语言选择按钮 -->
          <select></select>

            <!-- 演示按钮 -->
          <div class="controls">
              <button id="play" type="submit">Play</button>
          </div>
        </form>
    </div>
</template>
<script>
    import headTitle from '../../components/head-title.vue'
    import types from '../../store/mutation-types'
    import Util from '../../assets/lib/Util'
    import Tool from '../../assets/lib/Tool'
    import { XInput } from 'vux'
    export default {
        name: 'modify',
        data () {
            return {
                old_password_value: '',
                password_value: '',
                too_password_value: '',
                is_timer: false
            }
        },
        created () {
            
        },
        methods: {
            
        },
        components: {
            headTitle,
            XInput
        }
    }
</script>
<style lang="scss">
    @import "../../assets/scss/define";
    .speechJump2{
        position:absolute;
        bottom:33px;
        right:3px;
        padding:2px;
        width:40px;
        height:40px;
        background: green;
    }

    .txt, select, form > div {
    display: block;
    margin: 0 auto;
    font-family: sans-serif;
    font-size: 16px;
    padding: 5px;
    }

    .txt {
    width: 80%;
    }

    select {
    width: 83%;
    }

    form > div {
    width: 81%;
    }

    .txt, form > div {
    margin-bottom: 10px;
    overflow: auto;
    }

    .clearfix {
    clear: both;
    }

    label {
    float: left;
    width: 10%;
    line-height: 1.5;
    }

    .rate-value, .pitch-value {
    float: right;
    width: 5%;
    line-height: 1.5;
    }

    #rate, #pitch {
    float: right;
    width: 81%;
    }

    .controls {
    text-align: center;
    margin-top: 10px;
    }

    .controls button {
    padding: 10px;
    }
    
</style>