<template>
    <div>
        <div>帮助向导</div>
        <div ref="s1" class="button">操作按钮1</div>
        <div ref="s2" class="button" style="left: 300px;">操作按钮2</div>
        <div ref="s3" class="button" style="float:right;">操作按钮3</div>
        <div ref="s4" class="button" style="bottom:0;position: fixed;">操作按钮4</div>
        <ve-wizard :options="options"></ve-wizard>
    </div>
</template>

<script>
import  veWizard  from '../src'

export default {
    props:{},
    data(){
        return {
            options: {
                // 帮助按钮停靠位置
                position: "right",
                // 帮助按钮停靠位置修正参数
                /* top:'60px',
                   left:'10%',
                   right:'0px',
                   bottom:'0px', 
                */
                // 帮助按钮默认颜色
                background: "#830000",
                helpTags: "点击获取本页操作帮助",
                // 如果帮助是在模态框里的，需要根据不同的ui框架hack掉多余的相对距离
                // 默认值是element-ui的模态框
                hackDialogClassName: "",
                // 聚焦内容不可操作（只读观看模式）如果调整成可操作模式，有复杂z-index层级（比如模态框）可能出问题，需要做兼容性调试。
                disabled: true,
                // 帮助步骤数据，ref是调用父组件里的当前帮助步骤聚焦的元素ref绑定值
                helpData: [
                    {
                        title: "操作步骤1",
                        context: "聚焦内容不可操作（只读观看模式）如果调整成可操作模式，有复杂z-index层级（比如模态框）可能出问题，需要做兼容性调试。",
                        ref: "s1"
                    },
                    {
                        title: "操作步骤2",
                        context: "要给需要聚焦的操作区域标识上ref参数，并将参数写到options里面去",
                        ref: "s2"
                    },
                    {
                        title: "操作步骤3",
                        context: "然后配置上相应的文字说明，就可以愉快的玩耍了。",
                        ref: "s3"
                    },
                    {
                        title: "操作步骤4",
                        context: "箭头方向会自动转换，当然不排除在有些特定的dom树下面会出错，如果出错请到github来提交issues",
                        ref: "s4"
                    }
                ]
            },
        }
    },
    computed:{},
    watch:{},
    created(){},
    mounted(){},
    updated(){},
    methods:{},
    components:{
        veWizard
    },
}
</script>

<style scoped>
.button {
    position: relative;
    height: 68px;
    width: 120px;
}
</style>