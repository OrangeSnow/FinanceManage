<template>
    <div class="container-view">
        <head-title :title="'理财聊天室'"></head-title>
        <div class="chart-wrap" style="top:42px;">
            <scroller lock-x
                      height="-118"
                      :scrollbarY="true"
                      @on-scroll="onScroll"
                      ref="chartScrollEvent"
                      style=""
                      class="sss"
                      >
                <div class="chart-con-maid">
                    <div class="messageBox"></div>
                </div>
            </scroller>
        </div>
        <div class='inputBox' >
            <textarea class='messageText'></textarea>
            <!-- <input class='sendbtn' type="button" value='发送' v-on:click="SendBtn()" v-on:keyup.13="SendBtn"/> -->
            <mu-raised-button label="发送" class="demo-raised-button sendbtn" id="demo_change" v-on:click="SendBtn()" primary style=""/>
        </div>
        <!-- <div class="speechJump"><a href="#/speech">语音</a></div> -->
        <mu-float-button id="autoJump" icon="☺" class="demo-float-button" href="#/speech"/>
    </div>
</template>
<script>
    import types from '../../store/mutation-types'
    import headTitle from '../../components/head-title.vue'
    import Util from '../../assets/lib/Util'
    import Tool from '../../assets/lib/Tool'
    import bc from '../../../static/img/bc.jpg'
    import { Scroller } from 'vux'
    export default {
        name: 'chart',
        data () {
            return {
                consumption_chart_arr: [0,0,0,0,0,0,0,0],
                earn_chart_arr: [0,0,0],
                bc: bc
            }
        },
        components: {
            Scroller,
            headTitle
        },
        mounted () {
            this.StoreInit();
        },
        methods: {
            StoreInit(){
                //1、获取本地存储的对话内容
                    var storeChat;
                    if(sessionStorage.storeChat==undefined){
                        sessionStorage.storeChat="主人，萌萌酱正在为您服务！";
                        storeChat=sessionStorage.storeChat;
                    //初始问候声音
                        var soundstart = speechSynthesis.getVoices();
                        var meng = new window.SpeechSynthesisUtterance(storeChat);
                        meng.pitch = localStorage.speech_pitch;
                        meng.rate = localStorage.speech_rate;
                        window.speechSynthesis.speak(meng);
                    }
                    else{
                        storeChat=sessionStorage.storeChat;
                    }                
                //2、存储的字符串转化为数组
                    var storeChatArray=storeChat.split(',');   
                //3、遍历数组输出
                    for(var i=0;i<storeChatArray.length;i++){
                        if(i%2==1){
                            var createDivOld = document.createElement('div');
                            createDivOld.className = 'self';
                            createDivOld.innerText = storeChatArray[i];
                            // 添加到messageBox 中
                            document.querySelector('.messageBox').appendChild(createDivOld);


                        }
                        else{
                            var otherDivOld = document.createElement('div');
                            otherDivOld.className = 'other';
                            otherDivOld.innerText = storeChatArray[i];
                            // 添加到messageBox 中
                            document.querySelector('.messageBox').appendChild(otherDivOld);
                        }
                    }
            },
            SendBtn(){
                //4、对话内容
                    var storeChatString=sessionStorage.storeChat;
                // 获取 用户输入的 文本框的值
                    var inputValue = document.querySelector('.messageText').value;
                    storeChatString=storeChatString+","+inputValue;
                // 创造声音
                    var allsound = speechSynthesis.getVoices();

                // 创建 div  设置class 为 self,将输入的内容设置进去
                    var createDiv = document.createElement('div');
                    createDiv.className = 'self';
                    
                    createDiv.innerText = inputValue;
                // 添加到messageBox 中
                    document.querySelector('.messageBox').appendChild(createDiv); 
                    console.log(this.leftmoney);
                    var that=this;
                    setTimeout(function(){                      
                        var otherDiv = document.createElement('div');
                        otherDiv.className = 'other';

                        if(inputValue=="收支情况"){
                            if(localStorage.muchmoney<-100)
                                otherDiv.innerText = "我的天，主人，我实在不想讲出来，你非但没挣钱，还花了"+localStorage.muchmoney+"元！你要好好反省自己！";
                            else if(localStorage.muchmoney<0 && localStorage.muchmoney>-100)
                                otherDiv.innerText = "主人，你现在的收入是赤字。你让我怎么说你才好，竟然倒贴了"+localStorage.muchmoney+"元！";
                            else if(localStorage.muchmoney>0 && localStorage.muchmoney<200)
                                otherDiv.innerText = "主人，你还算努力，目前赚了"+localStorage.muchmoney+"元！";
                            else if(localStorage.muchmoney>300)
                                otherDiv.innerText = "哇，主人，萌萌酱为你开心，你竟然赚了"+localStorage.muchmoney+"元！";
                            else
                                otherDiv.innerText = "主人，你很佛系，没有挣到钱，但也没有花钱。";
                        }
                        else if(inputValue=="我想和你聊聊"){
                           otherDiv.innerText = "滚犊子，老娘不约！";
                        }
                        else if(inputValue=="我应该怎么省钱"){
                           otherDiv.innerText = "根据大数据分析，您总是买买买，您就是个败家子！";
                        }
                        else{
                           otherDiv.innerText = "主人你在说什么，我怎么听不懂？";
                        }

                    var utterThis = new window.SpeechSynthesisUtterance(otherDiv.innerText);
                    utterThis.pitch = localStorage.speech_pitch;
                    utterThis.rate = localStorage.speech_rate;
                    window.speechSynthesis.speak(utterThis);

                // 添加到messageBox 中おはようございます
                    document.querySelector('.messageBox').appendChild(otherDiv);
                    

                //5、字符串拼接
                    storeChatString=storeChatString+","+otherDiv.innerText;
                   
                //6、数组变字符串存入sessitionStorage
                    sessionStorage.storeChat=storeChatString;
                },300);
            },
            onScroll (pos) {
                this.scrollTop = pos.top;
            }
        }
    }
</script>
<style lang="scss">
    @import "../../assets/scss/define";
    .sss{
        background-color: #D1E9E9;
        background-image:url('../../../static/img/sun_rise.png');
        background-size: cover;
        padding:10px 15px;
    }
    .container {
        height: 100%;
        width: 100%;
        margin: 0 auto;
        flex-direction: column;
    }
          
    .messageBox {
        padding:5px 5px 103px;
        border-radius:6px;
        background-color:rgba(255,255,255,0.3);
        box-shadow: 0 0 0 1px hsla(0,0%,100%,.3) inset,0 .5em 1em rgba(0, 0, 0, 0.6);
        text-shadow: 0 1px 1px hsla(0,0%,100%,.3);
    }  
    .inputBox {
        position:absolute;
        bottom:0px;
        padding:2px;
        width:100%;
        height:40px;
        background: skyblue;
        overflow:hidden;
    }
    .speechJump{
        position:absolute;
        top:3px;
        right:3px;
        padding:2px;
        width:40px;
        height:40px;
        background: green;
    }  
    #autoJump{
        position:absolute;
        top:14px;
        right:5px;
    }   
    .messageText {
        font-size: 30px;
        font-size:16px;
        height:100%;
        width:75%;
    }
    .mu-ripple-wrapper{
        width:10px;
    }      
    .sendbtn {
        position:relative;
        display: inline-block;
        font-size: 14px;
        border-width: 0px;
        background: hotpink;
        top:-12px;
        text-align: center;
        width:10px;
    }
    #demo_change{
        min-width:0px;
        width: 65px;
        height:33px;
    }    
    .self {
        text-align: right;
        margin-top:5px;
        margin-left:273px;
        position:relative;  
        width:80px;  
        padding:9px 19px 9px 9px;  
        background:#F8C301;  
        border-radius:5px; /* 圆角 F8C301*/
        /* 
        border-top:1px solid  rgba(255,192,203,1);
        border-right:8px solid  rgba(2,19,203,1);
        border-bottom:1px solid  rgba(255,192,203,1);
        border-left:1px solid  rgba(255,192,203,1);
        background-image: url('../../../static/img/arrow_right.png');
        background-repeat: no-repeat;
        background-position: 123px 10px;
        */
    }
    .self::after{
        position:absolute;  
        top:8px;  
        right:-16px; /* 圆角的位置需要细心调试哦 */  
        width:0;  
        height:0;     
        border-top:8px solid  rgba(77,73,72,0);
        border-right:8px solid  rgba(77,73,72,0);
        border-bottom:8px solid  rgba(77,73,72,0);
        border-left:8px solid  rgba(255,192,203,1);
    }
         
    .other {
        margin-top:5px;
        position:relative; 
        margin-left:16px; 
        width:140px;  
        padding:9px 5px;  
        background:pink;  
        border-radius:5px; /* 圆角 */  
    }
    .other::after{  
        position:absolute;  
        top:8px;  
        left:-16px; /* 圆角的位置需要细心调试哦 */  
        width:0;  
        height:0;     
        border-top:8px solid  rgba(77,73,72,0);
        border-right:8px solid  rgba(255,192,203,1);
        border-bottom:8px solid  rgba(77,73,72,0);
        border-left:8px solid  rgba(77,73,72,0);
    } 
</style>
