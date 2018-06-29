<template>
    <div>
        <div class="outer-layer" :class="currentOptions.disabled==true?'disabled':'editor'"  v-show="currentValue===true">
            <!-- 这个是聚光灯焦点框 :style="'left:'+offsetLeft+'px;top:'+offsetTop+'px;width:'+offsetWidth+'px;height:'+offsetHeight+'px;'"-->
            <div class="spotlight" :class="currentOptions.disabled==true?'disabled':'editor'" :style="'left:'+offsetLeft+'px;top:'+offsetTop+'px;width:'+offsetWidth+'px;height:'+offsetHeight+'px;'">
            </div>

            <!-- 帮助信息框 -->
            <div class="help-info" 
                :style="'top:'+help_info_div_top()+'px;left:'+help_info_div_left()+'px;'">
                <div :class="'bubble-'+arrowDir+'-'+ arrowPosition" :style="'top:'+help_info_div_top()+'px;'"></div>
                <div class="info-title">
                    {{currentSubTitle}}
                    <span style="float: right;margin-right: 2rem;color: rgba(0,0,0,0.4);font-size: 14px;">{{currentModeName}}</span>
                </div>
                <div class="info-close" @click="currentValue=false" title="关闭向导">
                    <i class="iconfont icon-close"></i>
                </div>
                <div class="info-context" >
                    <slot name="help-context" :text="currentContext">
                        <div v-html="currentContext"></div>
                    </slot>
                </div>
                <div class="info-footer">
                    <button :disabled="lastIndex<=0" @click="GoToNext(-1)">上一步</button>
                    <button @click="currentValue=false">关闭</button>
                    <button :disabled="lastIndex+1>=currentOptions.helpData.length" style="float:right;" @click="GoToNext(1)">下一步</button>
                </div>
            </div>
        </div>

        <!-- 帮助精灵按钮 -->
        <div @click="currentValue=true" :title="currentOptions.helpTags" v-show="currentValue===false">
            <slot name="help-button"  >
                <div class="helper" 
                :title="currentOptions.helpTags" 
                :class="currentOptions.position" :style="
                    'top:'+currentOptions.top+';'+
                    'left:'+currentOptions.left+';'+
                    'right:'+currentOptions.right+';'+
                    'bottom:'+currentOptions.bottom+';'+
                    'background:'+currentOptions.background+';'
                    " @click="currentValue=true"  >
                        <span :style="'line-height: '+question_mark_height+';color: #fff;margin: 5px;'">
                            <!-- <i class="iconfont icon-help"></i> -->
                            ?
                        </span>
                </div>
            </slot>
        </div>
        

    </div>
</template>

<script>
export default {
    name: "ve-wizard",

    props: {
        // 传入参数
        options: {
            type: Object,
            default() {
                return null;
            }
        },
        // v-model绑定值，用于是否显示当前帮助页面
        value: {
            type: Boolean,
            default: false
        }
    },
    data() {
        return {
            thisChildrenIsWizard: true,
            currentValue: this.value,
            currentOptions: {},
            defaultOptions: {
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
                hackDialogClassName: "el-dialog",
                // 聚焦内容不可操作（只读观看模式）如果调整成可操作模式，有复杂z-index层级（比如模态框）可能出问题，需要做兼容性调试。
                disabled: true,
                // 帮助步骤数据，ref是调用父组件里的当前帮助步骤聚焦的元素ref绑定值
                helpData: [
                    {
                        title: "帮助子标题",
                        context: "帮助内容",
                        ref: null
                    }
                ]
            },
            currentSubTitle: "当前帮助标题",
            currentContext: "当前帮助内容",
            // 当前stepIndex索引
            lastIndex: null,
            // 上一个操作的聚焦框style
            lastPosition: "",
            lastZindex: 0,
            // 焦点位置和大小
            offsetTop: 0,
            offsetLeft: 0,
            offsetWidth: 0,
            offsetHeight: 0,
            currentModeName: "只读模式",
            // 箭头位置
            arrowPosition: "left",
            // 箭头方向
            arrowDir: "bottom"
        };
    },
    computed: {
        // 帮助按钮文字高度（根据停靠的位置计算）
        question_mark_height() {
            let h = "50px";
            if (
                this.currentOptions.position === "right" ||
                this.currentOptions.position === "left"
            ) {
                h = "50px";
            }

            if (
                this.currentOptions.position === "top" ||
                this.currentOptions.position === "bottom"
            ) {
                h = "25px";
            }
            return h;
        }
    },
    watch: {
        currentValue(value) {
            this.showStep(0);
            this.$emit("input", this.currentValue);
            this.$emit("change", this.currentValue);
            if (value == false) {
                this.changeLastFocusStyle();
            }
        },
        value(){
            this.currentValue = this.value
        },
        options() {
            this.initOptions(this.options);
        }
    },
    created() {
        this.initOptions(this.options);
    },
    mounted() {},
    updated() {},
    methods: {
        // 帮助信息框top
        help_info_div_top() {
            let allHeight = window.innerHeight;
            let top = this.offsetTop + this.offsetHeight + 50;
            if (top > allHeight / 2) {
                top = this.offsetTop - 240;
                this.arrowDir = "bottom";
            } else {
                this.arrowDir = "top";
            }
            return top;
        },
        help_info_div_left() {
            if (this.offsetLeft + 650 > window.innerWidth) {
                this.arrowPosition = "right";
                return this.offsetLeft - (650 - this.offsetWidth);
            } else {
                this.arrowPosition = "left";
                return this.offsetLeft;
            }
        },
        // 解析参数配置
        initOptions(propOptions) {
            let tem_ = {};
            let new_options = JSON.parse(JSON.stringify(this.defaultOptions));
            if (typeof propOptions == "string") {
                if (
                    propOptions.indexOf("{") >= 0 &&
                    propOptions.indexOf("}") >= 0
                ) {
                    tem_ = JSON.parse(propOptions);
                } else {
                    tem_ = {};
                }
            } else {
                tem_ = propOptions;
            }
            for (let key in tem_) {
                new_options[key] = tem_[key];
            }
            this.currentOptions = new_options;
        },

        // 跳动下一步或者上一步
        GoToNext(num) {
            if (
                num > 0 &&
                this.currentOptions.helpData.length > this.lastIndex + num
            ) {
                this.showStep(this.lastIndex + num);
            }

            if (num < 0 && this.lastIndex > 0) {
                this.showStep(this.lastIndex + num);
            }
        },
        // 显示当前帮助步骤
        showStep(sid) {
            function findParent(node) {
                // 查找父组件（有些ui会被包装两层）
                if (node.$parent && node.$parent.$children) {
                    for (let i in node.$parent.$children) {
                        if (node.$parent.$children[i].thisChildrenIsWizard) {
                            return node.$parent;
                        }
                    }
                }
                return findParent(node.$parent);
            }

            var parent = findParent(this);
            var thisRef;
            var self = this;
            // 获取绝对left
            function getElementLeft(element) {
                var actualLeft = element.offsetLeft;
                var current = element.offsetParent;
                while (current !== null) {
                    actualLeft += current.offsetLeft;
                    current = current.offsetParent;
                    // 如果帮助是在模态框里的，需要根据不同的ui框架hack掉多余的相对距离
                    if (
                        current !== null &&
                        current.className.indexOf(
                            self.currentOptions.hackDialogClassName
                        ) >= 0
                    ) {
                        break;
                    }
                }
                return actualLeft;
            }
            // 获取绝对top
            function getElementTop(element) {
                var actualTop = element.offsetTop;
                var current = element.offsetParent;
                while (current !== null) {
                    actualTop += current.offsetTop;
                    current = current.offsetParent;
                    // 如果帮助是在模态框里的，需要根据不同的ui框架hack掉多余的相对距离
                    if (
                        current !== null &&
                        current.className.indexOf(
                            self.currentOptions.hackDialogClassName
                        ) >= 0
                    ) {
                        break;
                    }
                }

                return actualTop;
            }

            if (
                typeof this.currentOptions === "object" &&
                typeof this.currentOptions.helpData === "object" &&
                this.currentOptions.helpData.length > sid
            ) {
                // 检查参数中是否有ref提示
                if (typeof this.currentOptions.helpData[sid].ref !== "string") {
                    this.currentContext = `帮助向导参数错误${
                        this.currentOptions.helpData[sid]
                    }的元素。`;
                    return;
                }
                thisRef = parent.$refs[this.currentOptions.helpData[sid].ref];

                if (!thisRef) {
                    this.currentContext = `没找到ref名字为${
                        this.currentOptions.helpData[sid].ref
                    }的元素。`;
                    return;
                }

                // 检查dom是否被vue包装还是原生dom
                if (parent.$refs[this.currentOptions.helpData[sid].ref].$el) {
                    thisRef =
                        parent.$refs[this.currentOptions.helpData[sid].ref].$el;
                } else {
                    thisRef =
                        parent.$refs[this.currentOptions.helpData[sid].ref];
                }

                // 调整焦点框位置
                this.offsetTop = getElementTop(thisRef);
                this.offsetLeft = getElementLeft(thisRef);
                this.offsetWidth = thisRef.offsetWidth;
                this.offsetHeight = thisRef.offsetHeight;

                // 调整聚焦位置到最上层，变得可操作。
                this.changeLastFocusStyle();
                if (this.currentOptions.disabled == false) {
                    this.lastPosition = thisRef.style.position;
                    this.lastZindex = thisRef.style.zIndex;
                    thisRef.style.position = "relative";
                    thisRef.style.zIndex = 10000;
                }

                this.currentSubTitle = this.currentOptions.helpData[sid].title;
                this.currentContext = this.currentOptions.helpData[sid].context;
                this.lastIndex = sid;
            }
        },

        changeLastFocusStyle() {
            var parent = this.$parent.$parent;
            if (this.currentOptions.disabled == false) {
                this.currentModeName = "引导操作模式";
                // 上一个调整的聚焦位置，调整回去。
                if (this.lastIndex !== null) {
                    let lastRef;
                    lastRef =
                        parent.$refs[
                            this.currentOptions.helpData[this.lastIndex].ref
                        ];
                    // 检查dom是否被vue包装还是原生dom
                    if (
                        parent.$refs[
                            this.currentOptions.helpData[this.lastIndex].ref
                        ].$el
                    ) {
                        lastRef =
                            parent.$refs[
                                this.currentOptions.helpData[this.lastIndex].ref
                            ].$el;
                    } else {
                        lastRef =
                            parent.$refs[
                                this.currentOptions.helpData[this.lastIndex].ref
                            ];
                    }
                    lastRef.style.position = this.lastPosition;
                    lastRef.style.zIndex = this.lastZindex;
                }
            } else {
                this.currentModeName = "只读观看模式";
            }
        }
    },
    components: {}
};
</script>

<style scoped>
@import "./style.css";

button {
    margin-left: 20px;
    height: 28px;
}

/* 遮罩层基础属性 */
.outer-layer {
    width: 100%;
    height: 100%;
    top: 0;
    left: 0;
    position: fixed;
    z-index: 9990;
}
/* 编辑模式遮罩层 */
.outer-layer.editor {
    background-color: rgba(0, 0, 0, 0.7);
}

/* 只读模式遮罩层 */
.outer-layer.disabled {
    background-color: none;
}

.spotlight.editor {
    transition: 0.4s cubic-bezier(1, -1, 0, 2);
    left: 210px;
    top: 840px;
    width: 1048px;
    height: 21px;
    position: absolute;
    background-color: rgba(255, 255, 255, 1);
    border-radius: 5px;
}

.spotlight.disabled {
    transition: 0.4s cubic-bezier(1, -1, 0, 2);
    position: fixed;
    border-radius: 5px;
    box-shadow: rgba(0, 0, 0, 0.8) 0px 0px 0px 2005px;
}

.help-info {
    transition: 0.4s cubic-bezier(1, -1, 0, 2);
    position: absolute;
    z-index: 9993;
    background-color: rgba(255, 255, 255, 1);
    height: 185px;
    width: 650px;
    border-radius: 10px;
}

.info-close {
    position: absolute;
    top: 5px;
    right: 10px;
    font-weight: 300;
    cursor: pointer;
}

.info-title {
    margin: 5px 10px;
    font-weight: 600;
}

.info-context {
    margin: 10px;
}

.info-footer {
    bottom: 0px;
    margin: 10px;
    position: absolute;
}

/* 三角形 */
.bubble-top-left:before {
    content: "";
    position: absolute;
    bottom: 100%;
    left: 20px;
    width: 0;
    height: 0;
    border-width: 11px;
    border-style: solid;
    border-color: transparent;
    border-bottom-width: 14px;
    border-bottom-color: #fff;
}

.bubble-bottom-left:before {
    content: "";
    position: absolute;
    top: 100%;
    left: 20px;
    width: 0;
    height: 0;
    border-width: 11px;
    border-style: solid;
    border-color: transparent;
    border-top-width: 14px;
    border-top-color: #fff;
}

.bubble-top-right:before {
    content: "";
    position: absolute;
    bottom: 100%;
    left: 600px;
    width: 0;
    height: 0;
    border-width: 11px;
    border-style: solid;
    border-color: transparent;
    border-bottom-width: 14px;
    border-bottom-color: #fff;
}

.bubble-bottom-right:before {
    content: "";
    position: absolute;
    top: 100%;
    left: 600px;
    width: 0;
    height: 0;
    border-width: 11px;
    border-style: solid;
    border-color: transparent;
    border-top-width: 14px;
    border-top-color: #fff;
}

.helper {
    position: fixed;
    z-index: 99999;
    cursor: pointer;
    box-shadow: 2px 4px 5px rgba(0, 0, 0, 0.3),
        0 0 40px rgba(0, 0, 0, 0.1) inset;
}

.helper:hover {
    box-shadow: 15px 15px 15px 15px rgba(0, 0, 0, 0.3),
        20px 20px 20px 20px rgba(0, 0, 0, 0.1) inset;
    transition: all 600ms cubic-bezier(0.68, -0.55, 0.265, 1.55);
}

.helper.right {
    border-radius: 50px 0px 0px 50px;
    right: 0;
    text-align: right;
    width: 25px;
    height: 50px;
    top: 90%;
}

.helper.left {
    border-radius: 0px 50px 50px 0px;
    left: 0;
    text-align: left;
    width: 25px;
    height: 50px;
    top: 90%;
}

.helper.top {
    border-radius: 0px 0px 50px 50px;
    right: 0px;
    text-align: center;
    width: 50px;
    height: 25px;
    top: 0px;
}

.helper.bottom {
    border-radius: 50px 50px 0px 0px;
    text-align: center;
    width: 50px;
    height: 25px;
    bottom: 0px;
}
</style>