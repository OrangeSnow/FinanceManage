<template>
        <div class="container">
            <div class="messageBox">
            </div>
            <div class='inputBox'>
                <textarea class='messageText'></textarea>
                <input class='sendbtn' value='发送' v-on:click="SendBtn()" v-on:keyup.13="SendBtn"/>
            </div>
        </div>

</template>

<script>
    import { Scroller, Spinner, XDialog} from 'vux'
    import GestureMobile from '../../assets/lib/GestureMobile'
    import types from '../../store/mutation-types'
    import CountUp from '../../assets/lib/countUp'
    import Util from '../../assets/lib/Util'
    import Tool from '../../assets/lib/Tool'
    export default {
        name: 'home',
        data: function () {
            return {
                is_open: false,
                total_balance: 0,
                is_popup: false
            }
        },
        components:{
            Scroller,
            Spinner,
            XDialog
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
                        //sessionStorage.storeChat="";
                        //storeChat="";
                    }
                    else{
                        storeChat=sessionStorage.storeChat;
                    }
                    console.log(sessionStorage.storeChat+"test");                   
                //2、存储的字符串转化为数组
                    var storeChatArray=storeChat.split(',');
                    console.log(storeChatArray);   
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
                    console.log(storeChatString);
				// 获取 用户输入的 文本框的值
				    var inputValue = document.querySelector('.messageText').value;
                    storeChatString=storeChatString+","+inputValue;
                    console.log(storeChatString);
				// 创造声音
				    var allsound = speechSynthesis.getVoices();

				// 创建 div  设置class 为 self,将输入的内容设置进去
				    var createDiv = document.createElement('div');
				    createDiv.className = 'self';
				    createDiv.innerText = inputValue;
                // 添加到messageBox 中
                    document.querySelector('.messageBox').appendChild(createDiv); 
				
                    setTimeout(function(){                      
                        var otherDiv = document.createElement('div');
                        otherDiv.className = 'other';
                        if(inputValue=="四月报表"){
                            otherDiv.innerText = "你四月第一次工作，挣了3000元哦！";
                        }
                        else if(inputValue=="五月报表"){
                           otherDiv.innerText = "五月你只工作了半个月，因此工资只有1500哦。";
                        }
                        else if(inputValue=="我想和你聊聊"){
                           otherDiv.innerText = "滚犊子，老娘不约！";
                        }
                        else if(inputValue.innerText=="我应该怎么省钱"){
                           otherDiv.innerText = "根据大数据分析，您总是买买买，您就是个败家子！";
                        }
                        else{
                           otherDiv.innerText = "主人你在说什么，我怎么听不懂？";
                        }

                    var utterThis = new window.SpeechSynthesisUtterance(otherDiv.innerText);
                    window.speechSynthesis.speak(utterThis);

                // 添加到messageBox 中
                    document.querySelector('.messageBox').appendChild(otherDiv);

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
        height: 100%;
        width: 100%;
        background: yellowgreen;
        overflow: scroll;
    }
          
    .inputBox {
        position:absolute;
        bottom:54px;
        padding:2px;
        width:100%;
        height:50px;
        background: skyblue;
        border:1px solid blue;
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
        height:49px;
        top:-21px;
        text-align: center;
    }
          
    .messageBox div {
        height: 30px;
        line-height: 30px;
    }
          
    .self {
        padding:2px 14px;
        height:60px;
        background: white;
        text-align: right;
    }
          
    .other {
        padding:2px 14px;
        background: pink;
    }
</style>
