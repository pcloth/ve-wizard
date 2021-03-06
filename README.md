# ve-wizard

[![npm](https://img.shields.io/npm/v/ve-wizard.svg) ![npm](https://img.shields.io/npm/dm/ve-wizard.svg)](https://www.npmjs.com/package/ve-wizard)
[![vue2](https://img.shields.io/badge/vue-2.x-brightgreen.svg)](https://vuejs.org/)

这是一个便于使用的帮助向导组件

## [English document by machine translation](./README.EN.MD)

# 例子

![demo](/images/demo.gif)


# 安装方法

```
npm install --save ve-wizard
```

## 全局导入
```javascript
import Vue from 'vue'
import veWizard from 've-wizard'

Vue.use(veWizard)
```

## 页面引入
``` html
<ve-wizard v-model="showHelp" :options="options"></ve-wizard>
```


## props参数
| 参数名 | 类型 | 默认值 | 说明 |
|---|---|---| --- |
| value | boolean | false | 是否显示向导窗体 |
| options | object | 见下表 | 配置向导参数 |


## options 配置表
| 参数名 | 类型 | 默认值 | 说明 |
|---|---|---| --- |
| position | string | right | 默认按钮位置，left/right |
| background | string | #830000 | 默认按钮背景颜色 |
| helpTags | string | 点击获取本页操作帮助 | 默认按钮的title提示 |
| hackDialogClassName | string | el-dialog | 当需要对模态框区域聚焦帮助向导的时候，有些ui框架的模态框会造成识别偏移值位置错误，需要指定ui的模态框父节点的className，默认是element-ui的模态框 |
| disabled | boolean | true | 聚焦后的操作区域禁止操作，如果允许操作，某些ui上可能显示错位 |
| helpData | array | [] | 帮助向导的内容数据，见下表 |

## options.helpData 配置说明
| 参数名 | 类型 | 默认值 | 说明 |
|---|---|---| --- |
| title | string | 无 | 当前向导步骤的标题 |
| context | string | 无 | 当前向导步骤的内容，支持html字符串。 |
| ref | string | 无 | 当前向导步骤需要聚焦的div所配置的ref名称。 |

## slot 插槽
| 插槽名称 | slot-scope参数 | 说明 |
|---|---|---|
| help-context | text | 当前向导步骤的内容区域插槽 |
| help-button | 无 | 帮助按钮插槽，当需要完全自定义样式的时候使用。 |


## 完整的demo代码
```html
<template>
    <div>
        <div>帮助向导</div>
        <div ref="s1" class="button">div 1</div>
        <div ref="s2" class="button" style="left: 300px;">div 2</div>
        <div ref="s3" class="button" style="float:right;">div 3</div>
        <div ref="s4" class="button" style="bottom:0;position: fixed;">div 4</div>
        <div class="button" style="bottom:0;right:80px;position: fixed;" @click="slot=!slot">
            自定义样式：{{slot}}  ---->
        </div>
        <ve-wizard v-model="showHelp" :options="options">
            <div slot="help-button" v-if="slot">
                <i class="iconfont icon-help my"></i>
            </div>
        </ve-wizard>
    </div>
</template>

<script>
import  veWizard  from 've-wizard'

export default {
    props:{},
    data(){
        return {
            showHelp:false,
            slot:false,
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
    border: 1px solid rgba(1,1,1,0.2);
    
}

.my {
    font-size: 36px;
    cursor: pointer;
    text-shadow: black 0.1em 0.1em 0.2em;
    position: fixed;
    right: 10px;
    bottom: 30px;
    color:#830000;
}
.my:hover {
    font-size: 40px;
    font-weight: 600;
}
</style>
```

## 许可

[MIT](http://opensource.org/licenses/MIT)
