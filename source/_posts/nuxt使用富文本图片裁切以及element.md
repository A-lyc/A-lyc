---
uuid: df3f33ae-ab7a-99bf-60a6-b5446f5ec0ad
title: nuxt使用富文本图片裁切以及element
date: 2020-06-24 09:49:59
tags: nuxt vue
category: nuxt vue
---
安装方法：npm install -d vue-quill-editor quill vue-cropper
官网：
vue-quill-editor：https://quilljs.com/ 
git：https://github.com/surmon-china/vue-quill-editor
vue-cropper：https://github.com/xyxiao001/vue-cropper

nuxt制作服务器端渲染造成的找不到window和document的问题，是由于有些插件是获取window和document的，在vue中是没有window和document的，所以官方给出了解决方案，
解决方案1：（局部组件使用 使用插件的时候不要使用，）
```js
if (process.client) {
  require('external_library')
}
```
解决方案2：（使用插件的时候全局注册一下）
注册插件：plugins文件夹内
```js
import Vue from 'vue'
import VueQuillEditor from 'vue-quill-editor'
Vue.use(VueQuillEditor)
```
全局引用：nuxt.config.js配置
```js
  plugins: [
    '@/plugins/element-ui',
    '@/plugins/bootstrap-vue',
    { src: '@/plugins/map', ssr: false },
    { src: '@/plugins/vue-quill-editor', ssr: false },
    { src: '@/plugins/vue-cropper', ssr: false }
  ],
```
使用：（不需要引入了，就像element一样，按照官网直接写标签，）
--  在使用上传的时候action是传输一个链接地址，如果想根据自己定义的axios上传需要添加一个auto-upload="false"意思不取消自动上传更据:on-change="onChange"(文件状态改变时的钩子，添加文件、上传成功和上传失败时都会被调用)动作进行上传
···下面代码网上摘得，可用但需要读
```html
<template>
  <div>
    <!-- 图片上传组件-->
    <el-upload
      accept="image/*"
      action="192.168.0.108/api/subject/file/upload"
      class="avatar-uploader"
      name="upload"
      :show-file-list="false"
      :before-upload="beforeUpload"
      hidden>
      <el-button size="small" type="primary">点击上传图片 到 文本编辑器</el-button>
    </el-upload>
    <!-- 编辑器组件-->
    <quill-editor
      class="editor"
      v-model="content"
      ref="myQuillEditor"
      :options="editorOption"
      @change="onEditorChange($event)">
    </quill-editor>

    <!-- 图片裁剪组件-->
    <el-dialog top="5vh" :visible.sync="isShowCropper">
      <VueCropper
        style="height:600px;margin:20px 0"
        ref="cropper"
        :img="option.img"
        :outputSize="option.outputSize"
        :outputType="option.outputType"
        :info="option.info"
        :canScale="option.canScale"
        :autoCrop="option.autoCrop"
        :autoCropWidth="option.autoCropWidth"
        :autoCropHeight="option.autoCropHeight"
        :fixed="option.fixed"
        :fixedNumber="option.fixedNumber"
      >
      </VueCropper>
      <br/>
      <el-button type="primary" @click="onCubeImg()">生成图片</el-button>
      <el-button @click="isShowCropper = false">取消</el-button>
    </el-dialog>
  </div>
</template>
<script>
  // 富文本工具栏配置
  const toolbarOptions = [
    ["bold", "italic", "underline", "strike"], // 加粗 斜体 下划线 删除线
    ["blockquote", "code-block"], // 引用  代码块
    [{ header: 1 }, { header: 2 }], // 1、2 级标题
    [{ list: "ordered" }, { list: "bullet" }], // 有序、无序列表
    [{ script: "sub" }, { script: "super" }], // 上标/下标
    [{ indent: "-1" }, { indent: "+1" }], // 缩进
    // [{'direction': 'rtl'}],                         // 文本方向
    [{ size: ["small", false, "large", "huge"] }], // 字体大小
    [{ header: [1, 2, 3, 4, 5, 6, false] }], // 标题
    [{ color: [] }, { background: [] }], // 字体颜色、字体背景颜色
    [{ font: [] }], // 字体种类
    [{ align: [] }], // 对齐方式
    ["clean"], // 清除文本格式
    ["link", "image", "video"] // 链接、图片、视频
  ];

//单独引入编辑器的css - 以免冲突
  import "quill/dist/quill.core.css";
  import "quill/dist/quill.snow.css";
  import "quill/dist/quill.bubble.css";

  export default {
    name: 'textEditor',
    components: {
    },
    props: {
      /*编辑器的内容*/
      value: {
        type: String
      },
      /*图片大小*/
      maxSize: {
        type: Number,
        default: 4000 //kb
      }
    },
    data() {
      return {
        // 富文本数据
        content: '',
        quillUpdateImg: false, // 根据图片上传状态来确定是否显示loading动画，刚开始是false,不显示
        editorOption: {
          theme: "snow", // or 'bubble'
          placeholder: "请输入您想输入的内容",
          modules: {
            toolbar: {
              container: toolbarOptions,
              // container: "#toolbar",
              //触发富文本上的点击动作
              handlers: {
                image: function(value) {
                  console.log(value)
                  if (value) {
                    // 触发input框选择图片文件
                    document.querySelector(".avatar-uploader input").click();
                  } else {
                    this.quill.format("image", false);
                  }
                },
              }
            }
          }
        },
        // 切图器数据
        option: {
          img: '',                         // 裁剪图片的地址
          info: true,                      // 裁剪框的大小信息
          outputSize: 1,                   // 裁剪生成图片的质量
          outputType: 'png',               // 裁剪生成图片的格式
          canScale: false,                 // 图片是否允许滚轮缩放
          autoCrop: true,                  // 是否默认生成截图框
          autoCropWidth: 150,              // 默认生成截图框宽度
          autoCropHeight: 150,             // 默认生成截图框高度
          fixed: false,                    // 是否开启截图框宽高固定比例
          fixedNumber: [4, 4],             // 截图框的宽高比例
        },
        isShowCropper: false,
        isClient:true,
      };
    },
    methods: {
      // 上传切图前调用
      beforeUpload(file) {
        this.option.img = URL.createObjectURL(file);
        this.option.autoCropWidth = this.width;
        this.option.autoCropHeight = this.height;
        this.isShowCropper = true;
        return false;
      },
      // 确定裁剪图片
      onCubeImg() {
        // 获取cropper的截图的base64 数据
        this.$refs.cropper.getCropData(data => {
          this.isShowCropper = false
          // 先将显示图片地址清空，防止重复显示
          this.option.img = ''
          // 将剪裁后base64的图片转化为file格式
          let file = this.convertBase64UrlToBlob(data)
          file.name = 'img' + new Date().getTime();
          // 将剪裁后的图片执行上传
          var fd = new FormData();
          fd.append("file", file);
          this.$message.success("图片正在上传");
          ApiFileUploadFile(fd).then(res => {
            // 获取富文本组件实例
            this.$message.success("图片上传成功");
            let quill = this.$refs.myQuillEditor.quill;
            // loading动画消失
            this.quillUpdateImg = false;
            // 如果上传成功
            if (res.data.code == 0) {
              // 获取光标所在位置
              if (quill.getSelection()&&quill.getSelection().index) {
                let length = quill.getSelection().index;
              }else{
                let length = 0;
              }
              // 插入图片  res.url为服务器返回的图片地址
              console.log(res)
              quill.insertEmbed(length, "image", res.data.data.imageUrl);
              // 调整光标到最后
              quill.setSelection(length + 1);
            } else {
              this.$message.error("图片插入失败");
            }
          })
        })
      },
      // 将base64的图片转换为file文件
      convertBase64UrlToBlob(urlData) {
        let bytes = window.atob(urlData.split(',')[1]);//去掉url的头，并转换为byte
        //处理异常,将ascii码小于0的转换为大于0
        let ab = new ArrayBuffer(bytes.length);
        let ia = new Uint8Array(ab);
        for (var i = 0; i < bytes.length; i++) {
          ia[i] = bytes.charCodeAt(i);
        }
        return new Blob([ab], { type: 'image/png' });
      },
      onEditorChange() {
        //富文本内容改变事件
        this.$emit("change", this.content);
      },
    },
    mounted() {
      setTimeout(() => {
        this.content = this.value
      },500)
    },
  };
</script>
<style lang="scss">

</style>
```


