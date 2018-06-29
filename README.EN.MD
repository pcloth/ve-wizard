# ve-wizard

[![npm](https://img.shields.io/npm/v/ve-wizard.svg) ![npm](https://img.shields.io/npm/dm/ve-wizard.svg)](https://www.npmjs.com/package/ve-wizard)
[![vue2](https://img.shields.io/badge/vue-2.x-brightgreen.svg)](https://vuejs.org/)

This is an easy-to-use help wizard component

# example

![demo](/images/demo.gif)


# installation

```
npm install --save ve-wizard
```

## Global import
```javascript
import Vue from 'vue'
import veWizard from 've-wizard'

Vue.use(veWizard)
```

## Page introduction
``` html
<ve-wizard v-model="showHelp" :options="options"></ve-wizard>
```


## props
| name | type | default value | description |
|---|---|---| --- |
| value | boolean | false | Whether to display the wizard dialog |
| options | object | See next Table | Configuration Wizard Parameters |


## options configuration table
| name | type | default value | description |
|---|---|---| --- |
| position | string | right | Default button position： left/right |
| background | string | #830000 | default button background color |
| helpTags | string | 点击获取本页操作帮助 | Default Button Title Tip |
| hackDialogClassName | string | el-dialog | When the modal box area needs to be focused on the help wizard, some modal boxes of the UI framework will cause the location of the recognition offset value to be wrong. You need to specify the class of the modal box parent element-ui. Name, the default is the modal box of the element-ui |
| disabled | boolean | true | The operation area after the focus is forbidden. If it is allowed to operate, some ui may show misplaced |
| helpData | array | [] | Help Wizard content data, see next table |

## options.helpData
| name | type | default value | description |
|---|---|---| --- |
| title | string | 无 | Title of current wizard step |
| context | string | 无 | The content of the current wizard step supports html strings. |
| ref | string | 无 | current wizard step div which requires focus all references to the configuration. |

## slot
|Slot Name | slot-scope Parameters | Description |
|---|---|---|
| help-context | text | Content Area Slots for Current Wizard Steps |
| help-button | none | Help Button Slot, used when full custom style is required. |


## full demo code
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
