<!--Created by 337547038 on 2017/12/14.-->
<!--copy on pc-->
<!--example
<InputNumber v-model="value" :max="10"></InputNumber>-->
<template>
    <div class="input-number">
        <a href="javascript:;" class="icon-less" @click="_numberControl(-1)"
           :class="{'disabled':inputValue<=min,'disabled':disabled}"></a>
        <input type="number" class="input-control" v-model="inputValue" :class="{'disabled':disabled}"
               :disabled="disabled">
        <a href="javascript:;" class="icon-add" @click="_numberControl(1)"
           :class="{'disabled':inputValue>=max,'disabled':disabled}"></a>
    </div>
</template>
<script>
    export default {
        name: 'inputNumber',
        data(){
            return {
                inputValue: this.value
            }
        },
        watch: {
            inputValue(value){
                let val = Number(value);
                if (this.disabled) {
                    this.inputValue = this.value;
                    return false
                }
                if (!isNaN(val)) {
                    if (val > this.max) {
                        this.inputValue = this.max;
                    } else if (val < this.min) {
                        this.inputValue = this.min;
                    }
                    this.$emit('input', val);
                    this.change ? this.change(val) : ""
                } else {
                    this.inputValue = this.value;
                    this.$dialog({content: '请输入数字'});
                }
            }
        },
        props: {
            value: {
                type: Number,
                default: 1
            },
            max: {
                type: Number,
                default: 0
            },
            min: {
                type: Number,
                default: 0
            },
            step: {//每次增加或减少的数
                type: Number,
                default: 3
            },
            disabled: {
                type: Boolean,
                default: false
            },
            change: Function
        },
        mounted(){

        },
        components: {},
        methods: {
            _numberControl(type){
                let inputValue = Number(this.inputValue);
                if (!isNaN(inputValue && !this.disabled)) {
                    if (type > 0 && inputValue < this.max) {
                        //add
                        if ((inputValue + this.step) < this.max) {
                            this.inputValue = inputValue + this.step;
                        }
                        else {
                            this.inputValue = this.max;
                        }
                    } else if (type < 0 && inputValue > this.min) {
                        //less
                        if ((inputValue - this.step) > this.min) {
                            this.inputValue = inputValue - this.step;
                        } else {
                            this.inputValue = this.min;
                        }
                    }
                }
            }
        },
        computed: {}
    }
</script>