<template>
    <div class="consumption-wrap" id="cos_bc">
        <head-title :title="'消费'"></head-title>
        <ul class="input-warp233">
            <li class="input-item input-required">
                <calendar v-model="date_value" title="日期：" disable-future></calendar>
            </li>
            <li class="input-item input-required">
                <datetime
                    title="时间："
                    v-model="time_value"
                    format="HH:mm"
                    confirm-text="完成"
                    cancel-text="取消">
                </datetime>
            </li>
            <li class="input-item input-required">
                <popup-picker
                    title="消费类型："
                    :data="account_type_arr"
                    v-model="account_type">
                </popup-picker>
            </li>
            <li class="input-item input-required">
                <x-input id="jinge" v-model="sum_value" title="金额（￥）：" keyboard="number"></x-input>
            </li>
            <li class="input-item">
                <x-input id="beizhu" v-model="remarks_value" title="自定义："></x-input>
            </li>
        </ul>
        <i id="cos_btn" class="sure-btn" @click="subBill()" :class="{'sure-active-true':sum_value}">确 认</i>
        <mu-float-button id="jumpA" icon="回" class="demo-float-button" href="#/account"/>
    </div>
</template>
<script>
    import { PopupPicker, Datetime, Calendar, XInput } from 'vux'
    import headTitle from '../../../components/head-title.vue'
    import Tool from '../../../assets/lib/Tool'
    import Util from '../../../assets/lib/Util'
    import types from '../../../store/mutation-types'
    export default {
        name: 'consumption',
        created () {
            this.setNavIndex();
        },
        components: {
            Calendar,
            Datetime,
            XInput,
            PopupPicker,
            headTitle
        },
        data () {
            return {
                is_btn_active: false,
                sum_value: '',
                remarks_value: '',
                account_type_arr: [['水果零食','餐饮伙食', '出行旅游', '网上购物', '生活日常', '租房水电', '医疗药物','其它消费']],
                account_type:['水果零食'],
                date_value: 'TODAY',
                time_value: Tool.format('hh:mm'),
                is_timer: false
            }
        },
        methods: {
            /**提交账单*/
            subBill () {
                if(!this.sum_value) {
                    this.showMsg('请填写消费金额');
                    return;
                }
                if(this.is_timer) return;
                this.is_timer = true;
                this.$vux.loading.show({text:'Loading'});
                var bill = {
                    sum_value: this.sum_value,
                    date_value: this.date_value,
                    time_value: this.time_value,
                    remarks_value: this.remarks_value,
                    account_type: this.account_type,
                    type_number: this.billTypeNumber(this.account_type),
                    consumption_or_earn: 0,
                    user_name: Tool.dataToSessionStorageOperate.achieve('user').user_name
                };
                Util.addBill(bill, (result) => {
                    setTimeout(() => {
                        this.$vux.loading.hide();
                        this.is_timer = false;
                        if (result.status == 1) {
                            this.resetValue();
                        }
                        this.showMsg(result.msg);
                    },100)
                });
            },
            showMsg (msg) {
                this.$vux.toast.show({
                    type: 'text',
                    width: 'auto',
                    text: msg,
                    position: 'top'
                });
            },
            resetValue () {
                this.sum_value = '';
                this.remarks_value = '';
                this.date_value = 'TODAY';
                this.time_value = Tool.format('hh:mm');
            },
            setNavIndex () {
                this.$store.commit(types.SET_NAV_INDEX,'2')
            },
            billTypeNumber ( account_type ) {
                switch (account_type[0]){
                    case '水果零食':
                        account_type = 'sgls';
                        break;
                    case '餐饮伙食':
                        account_type = 'cyhs';
                        break;
                    case '出行旅游':
                        account_type = 'cxly';
                        break;
                    case '网上购物':
                        account_type = 'wsgw';
                        break;
                    case '生活日常':
                        account_type = 'shrc';
                        break;
                    case '租房水电':
                        account_type = 'cfsd';
                        break;
                    case '医疗药物':
                        account_type = 'ylyw';
                        break;
                    case '其它消费':
                        account_type = 'qt';
                        break;
                }
                return account_type;
            }
        }
    }
</script>
<style lang="scss">
@import "../../../assets/scss/define";
    #cos_bc{
        background-image:url('../../../../static/img/star.jpg');
        background-size: cover;
        bottom:0px;
    }
    #jumpA{
        position:absolute;
        top:70%;
        right:45%;
    } 
    .input-warp233{
        background-color: white;
        width:88%;
        margin:27px auto;
        border-radius: 12px;
        padding:45px 10px;
        box-shadow: 0 0 0 1px hsla(0,0%,100%,.3) inset,0 .5em 1em rgba(10, 10, 10, 0.6);
        border:10px solid #ee4f4f;
    }
    
    #cos_btn{
        width:55%;
        height:50px;
        line-height: 50px;
        margin:-53px auto;
        border-radius: 25px;
        background-color: #ee4f4f;
        font-size:23px;
        box-shadow: 0px 8px 3px #3f3f3f;
    }
    #jinge input{
        color:#999;
    }
    #beizhu input{
        color:#999;
    }
</style>