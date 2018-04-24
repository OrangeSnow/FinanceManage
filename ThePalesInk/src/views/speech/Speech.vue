<template>
    <div class="container-view login-wrap">
        <head-title :title="'修改密码：'"></head-title>
        <!-- <div class="speechJump2"><a href="#/maid">聊天室</a></div> -->
        <mu-float-button icon="star"  id="demoJump" class="demo-float-button" href="#/maid"/>
        <form>
            <input type="text" class="txt">
            <!-- 频率调节 -->
            <div>
                <label for="rate">频率</label><input type="range" min="0.5" max="2" value="1" step="0.1" id="rate">
                <div class="rate-value">1</div>
                <div class="clearfix"></div>
            </div>
            <!-- 音调高低调节 -->
            <div>
                <label for="pitch">音调</label><input type="range" min="0" max="2" value="1" step="0.1" id="pitch">
                <div class="pitch-value">1</div>
                <div class="clearfix"></div>
            </div>
            <!-- 语言选择按钮 -->
          <!-- <select></select> -->

            <!-- 演示按钮 -->
          <div class="controls">
              <button id="play" type="submit" v-on:click="inithua()">测试</button>
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
        methods: {
            inithua(){
                //创建声音
	            var synth = window.speechSynthesis;
                //获取表格
	            var inputForm = document.querySelector('form');
	            //获取输入的文字
                var inputTxt = document.querySelector('.txt');
                //音调高低拉条
	            var pitch = document.querySelector('#pitch');
	            //高低的1
	            var pitchValue = document.querySelector('.pitch-value');
	            //频率高低拉条
	            var rate = document.querySelector('#rate');
	            //频率的1
                var rateValue = document.querySelector('.rate-value');
                
                //最最核心就是下面四段代码，获取输入内容，改频率和音调
                var utterThis = new SpeechSynthesisUtterance(inputTxt.value);
	            utterThis.pitch = pitch.value;
	            utterThis.rate = rate.value;
                synth.speak(utterThis);
                
                //声音高低和频率存在localStorage里面，在Maid.vue里读取
                localStorage.speech_pitch = pitch.value;
                localStorage.speech_rate = rate.value;

                //改掉值
                 pitchValue.textContent = pitch.value;
                 rateValue.textContent = rate.value;
            }
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
    #demoJump{
        position:absolute;
        bottom:33px;
        right:3px;
    }
    .txt, select, form > div {
    display: block;
    margin: 0 auto;
    font-family: sans-serif;
    font-size: 16px;
    padding: 5px;
    }

    .txt {
    width: 85%;
    }

    select {
    width: 83%;
    }

    form > div {
    width: 85%;
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