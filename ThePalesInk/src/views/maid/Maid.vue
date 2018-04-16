<template>
    <div class="container-view">
        

        <div class="container">
                    <div class="messageBox">
                    </div>
                    <div class='inputBox'>
                              <input type="text" class='messageText'>
                              <input type="button" class='sendBtn' value='发送'>
                    </div>
          </div>


    </div>
</template>
<script>
    document.querySelector('.sendBtn').onclick = function(event) {

              // 获取 用户输入的 文本框的值
              var inputValue = document.querySelector('.messageText').value;

              // 创建 div  设置class 为 self  将 输入的内容 设置进去
              var createDiv = document.createElement('div');
              createDiv.className = 'self';
              createDiv.innerText = inputValue;
              // 添加到messageBox 中
              document.querySelector('.messageBox').appendChild(createDiv);                       
              var otherDiv = document.createElement('div');
              otherDiv.className = 'other';
              if(createDiv.innerText=="四月报表"){
              otherDiv.innerText = "你四月第一次工作，挣了3000元哦！";
              }
              else if(createDiv.innerText=="五月报表"){
                otherDiv.innerText = "五月你只工作了半个月，因此工资只有1500哦。";
              }
              else{
                otherDiv.innerText = "主人你在说什么，我怎么听不懂？";
              }
              // 添加到messageBox 中
              document.querySelector('.messageBox').appendChild(otherDiv);
    }
</script>
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
        created () {
            this.fetchBalance();
            this.gestureMobile();
            this.setNavIndex();
        },
        methods: {
            /**安全退出*/
            safeExit () {
                this.is_popup = false;
                Tool.dataToSessionStorageOperate.clear();
                this.$router.push('/login');
            },
            /**手势*/
            gestureMobile () {
                this.$nextTick(() => {
                    let _this = this;
                    GestureMobile(this.$el,{
                        upCallBackFun () {
                            _this.is_open = true;
                        },
                        downCallBackFun () {
                            _this.is_open = false;
                        }
                    });
                })
            },
            /**设置导航条按钮状态*/
            setNavIndex () {
                this.$store.commit(types.SET_NAV_INDEX,'1')
            }
        }
    }
</script>
<style lang="scss">
    @import "../../assets/scss/define";
    .home-active{
        .balance-wrap{
            top: 35%;
        }
        .home-btn-wrap{
            bottom: 15%;
            opacity: 1;
        }
    }
    .balance-wrap{
        @extend %pa;
        @extend %oh;
        @extend %l50;
        @extend %t50;
        width: 60%;
        transition: all 1s;
        transform: translate3d(-50%,-50%,0);
        padding-bottom: 60%;
        border-radius: 50%;
        background-color: #fff;
        border: 1px solid rgb(224, 230, 237)
    }
    .balance-title{
        @extend %pa;
        @extend %f12;
        @extend %fwn;
        @extend %tac;
        @extend %l0;
        @extend %r0;
        color: #999;
        top: 30%;
    }
    @keyframes arrow-animate {
        0%{
            bottom: 10px;
        }
        50%{
            bottom: 20px;
        }
        100%{
            bottom: 10px;
        }
    }




     html,
          body {
                    height: 100%;
                    margin: 0;
          }
          
          .container {
                    display: flex;
                    height: 100%;
                    border: 1px solid #0094ff;
                    width: 70%;
                    margin: 0 auto;
                    flex-direction: column;
          }
          
          .messageBox {
                    flex: 9;
                    background: yellowgreen;
                    overflow: scroll;
          }
          
          .inputBox {
                    flex: 2;
                    background: skyblue;
                    display: flex;
          }
          
          .messageText {
                    flex: 8;
                    font-size: 30px;
          }
          
          .sendBtn {
                    flex: 2;
                    font-size: 30px;
                    border-width: 0px;
                    background: hotpink;
          }
          
          .messageBox div {
                    height: 30px;
                    border: 1px solid #0094ff;
                    line-height: 30px;
          }
          
          .self {
                    background: pink;
          }
          
          .other {
                    text-align: right;
                    background: white;
          }
</style>
