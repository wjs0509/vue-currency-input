# vue-currency-input
基于vue+element的金额格式化组件

<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.5.17/dist/vue.js"></script>
    <!-- <script src="./accounting.js"></script> -->
    <script src="https://cdn.bootcss.com/accounting.js/0.4.1/accounting.js"></script>
    <!-- 引入样式 -->
    <link rel="stylesheet" href="https://unpkg.com/element-ui/lib/theme-chalk/index.css">
    <!-- 引入组件库 -->
    <script src="https://unpkg.com/element-ui/lib/index.js"></script>
</head>

<body>
    <div id="app">
        <div style="display:inline-block">
            <currency-input v-model="price" :decimal="4" style="width:200px;"></currency-input>
            <el-button type="primary" size="small" @click="cc">提交</el-button>
        </div>
    </div>
</body>

</html>
<script>
    Vue.component("currency-input", {
        template: '\
            <div class="el-input el-input--small">\
                <input class="el-input__inner"\
                    v-bind:value="formatValue"\
                    ref="input"\
                    v-on:input="updatevalue($event.target.value)"\
                    v-on:blur="onBlur"\
                    v-on:focus="selectAll"/>\
            </div>\
        ',
        props: {
            value: {
                type: [String, Number],
                default: 0,
                desc: '数值'
            },
            symbol: {
                type: String,
                default: '',
                desc: '货币标识符'
            },
            decimal: {
                type: Number,
                default: 2,
                desc: '小数位'
            }
        },
        data() {
            return {
                focused: false,
            }
        },
        computed: {
            formatValue() {
                if (this.focused) {
                    return accounting.unformat(this.value);
                } else {
                    return accounting.formatMoney(this.value, this.symbol, this.decimal);
                }
            }
        },
        methods: {
            updatevalue(value) {
                var formatvalue = accounting.unformat(value);
                this.$emit("input", formatvalue)
            },
            onBlur() {
                this.focused = false;
            },
            selectAll(event) {
                this.focused = true;
                setTimeout(() => {
                    event.target.select()
                }, 0)
            }
        },
    })
    new Vue({
        el: "#app",
        data() {
            return {
                price: ""
            }
        },
        created() {
            setTimeout(() => {
                this.price = 1100;
            }, 1000);
        },
        methods: {
            cc() {
                console.log(this.price)
            }
        },
    })
</script>
