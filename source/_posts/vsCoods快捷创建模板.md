---
uuid: f15e10b4-7b04-ff6c-b1a1-a6b987910e6a
title: vsCoods快捷创建模板
date: 2020-03-24 18:42:04
tags: vue
category: vue知识点
---
> 在一个Vue的项目中，反复的新建.vue文件是一个必不可少的工序。本着科技让人偷懒的原则，我们可以利用VSCode的snippet在.vue文件创建后能轻松地生成一套模板。

####    第一步：
使用ctrl+shift+p（文件-首选项-用户代码片段）输入vue 自动打开vue.json 的文件输入模板
模板具体格式：
$0  鼠标在的位置

```
 "Print to console": {
        "prefix": "vue",
        "body": [
            "<!-- $1 -->",
            "<template>",
            "<div class='${2:webapp}'>$5</div>",
            "</template>",
            "",
            "<script>",
            "//这里可以导入其他文件（比如：组件，工具js，第三方插件js，json文件，图片文件等等）",
            "//例如：import 《组件名称》 from '《组件路径》';",
            "",
						"export default {",
						"name:'${2:another}',",
            "//import引入的组件需要注入到对象中才能使用",
            "components: {},",
            "data() {",
            "//这里存放数据",
            "return {",
            "",
            "};",
            "},",
            "//监听属性 类似于data概念",
            "computed: {},",
            "//监控data中的数据变化",
            "watch: {},",
            "//方法集合",
            "methods: {",
            "",
            "},",
            "//生命周期 - 创建完成（可以访问当前this实例）",
            "created() {",
            "",
            "},",
            "//生命周期 - 挂载完成（可以访问DOM元素）",
            "mounted() {",
            "",
            "},",
            "beforeCreate() {}, //生命周期 - 创建之前",
            "beforeMount() {}, //生命周期 - 挂载之前",
            "beforeUpdate() {}, //生命周期 - 更新之前",
            "updated() {}, //生命周期 - 更新之后",
            "beforeDestroy() {}, //生命周期 - 销毁之前",
            "destroyed() {}, //生命周期 - 销毁完成",
            "activated() {}, //如果页面有keep-alive缓存功能，这个函数会触发",
            "}",
            "</script>",
            "<style lang='scss' scoped>",
            "//@import url($3); 引入公共css类",
            ".${2:another} {}",
            "</style>"
        ],
        "description": "Log output to console"
    }
```

####  第二步: 添加配置，让vscode允许自定义的代码片段提示出来
### 文件 --> 首选项 --> 设置 ---> 添加这2项
##  // Specifies the location of snippets in the suggestion widget
# "editor.snippetSuggestions": "top",
##  // Controls whether format on paste is on or off
# "editor.formatOnPaste": true