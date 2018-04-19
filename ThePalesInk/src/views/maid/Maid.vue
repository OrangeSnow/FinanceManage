<template>
    <div class="container-view">
        <head-title :title="'聊天室233'"></head-title>
        <div class="chart-wrap">
            <scroller lock-x
                      height="-118"
                      :scrollbarY="true"
                      @on-scroll="onScroll"
                      ref="chartScrollEvent" style="background-color: yellow;padding-bottom:50px;">
                <div class="chart-con">
                    <div class="messageBox"></div>
                </div>
            </scroller>
        </div>
        <div class='inputBox' >
            <textarea class='messageText'></textarea>
            <input class='sendbtn' type="button" value='发送' v-on:click="SendBtn()" v-on:keyup.13="SendBtn"/>
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

                            //箭头
                            var createDivOld2 = document.createElement('div');
                            createDivOld2.className = 'arrow';
                            document.querySelector('.self').appendChild(createDivOld2);
                        }
                        else{
                            var otherDivOld = document.createElement('div');
                            otherDivOld.className = 'other';
                            otherDivOld.innerText = storeChatArray[i];
                            // 添加到messageBox 中
                            document.querySelector('.messageBox').appendChild(otherDivOld);

                            //箭头
                            var otherDivOld2 = document.createElement('div');
                            otherDivOld2.className = 'arrow2';
                            document.querySelector('.other').appendChild(otherDivOld2);
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

                //箭头
                    var createDiv2 = document.createElement('div');
                    createDiv2.className = 'arrow';
                    document.querySelector('.self').appendChild(createDiv2);

                    setTimeout(function(){                      
                        var otherDiv = document.createElement('div');
                        otherDiv.className = 'other';
                //箭头
                        var otherDiv2 = document.createElement('div');
                        otherDiv2.className = 'arrow2';

                        if(inputValue=="四月报表"){
                            otherDiv.innerText = "你四月第一次工作，挣了3000元哦！";
                        }
                        else if(inputValue=="五月报表"){
                           otherDiv.innerText = "五月你只工作了半个月，因此工资只有1500哦。";
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
                    window.speechSynthesis.speak(utterThis);

                // 添加到messageBox 中
                    document.querySelector('.messageBox').appendChild(otherDiv);
                    
                //箭头
                    document.querySelector('.other').appendChild(otherDiv2);

                //5、字符串拼接
                    storeChatString=storeChatString+","+otherDiv.innerText;
                   
                //6、数组变字符串存入sessitionStorage
                    sessionStorage.storeChat=storeChatString;
                },300);
            }
        }
    }
</script>
<style lang="scss">
    @import "../../assets/scss/define";

    .container {
        height: 100%;
        width: 100%;
        margin: 0 auto;
        flex-direction: column;
    }
          
    .messageBox {
        padding:5px 5px 103px;
        background: yellowgreen;
    }    
    .inputBox {
        position:absolute;
        bottom:0px;
        padding:2px;
        width:100%;
        height:40px;
        background: skyblue;
        border:1px solid blue;
        overflow:hidden;
    }     
    .messageText {
        font-size: 30px;
        font-size:16px;
        height:100%;
        width:75%;
    }
          
    .sendbtn {
        position:relative;
        display: inline-block;
        font-size: 14px;
        border-width: 0px;
        background: hotpink;
        width:22%;
        height:40px;
        top:-16px;
        text-align: center;
    }
          
    .self {
        text-align: right;
        margin-top:5px;
        margin-left:253px;
        position:relative;  
        width:100px;  
        padding:9px 9px;  
        background:#F8C301;  
        border-radius:5px; /* 圆角 */  
    }
          
    .other {
        margin-top:5px;
        position:relative; 
        margin-left:10px; 
        width:150px;  
        padding:9px 5px;  
        background:pink;  
        border-radius:5px; /* 圆角 */  
    }
    .arrow {  
        position:absolute;  
        top:8px;  
        right:-16px; /* 圆角的位置需要细心调试哦 */  
        width:0;  
        height:0;     
        border-top:8px solid  rgba(77,73,72,0);
        border-right:8px solid  rgba(77,73,72,0);
        border-bottom:8px solid  rgba(77,73,72,0);
        border-left:8px solid  rgba(248,195,1,1);
    } 
    .arrow2 {  
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
