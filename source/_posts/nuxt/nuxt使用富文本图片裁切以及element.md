---
uuid: df3f33ae-ab7a-99bf-60a6-b5446f5ec0ad
title: nuxt使用富文本图片裁切以及element
date: 2020-06-24 09:49:59
tags: nuxt vue 使用富文本图片裁切以及element
category: Nuxt
---
问题点 - 为解决待测试
把后台接收的THML重新渲染到富文本上，做到一个重新修改的作用，思路：ref直接父组件修改子组件的值
问题，id需要传入之后提前渲染页面 
<!-- more -->
父组件使用 @change - <editor @change="contentEdit" ref="edit" id="quill-input-all">

之前默认安装element
安装方法安装富文本图片裁切：npm install -d vue-quill-editor quill vue-cropper 
官网：
vue-quill-editor：https://quilljs.com/ 
git：https://github.com/surmon-china/vue-quill-editor
vue-cropper：https://github.com/xyxiao001/vue-cropper

nuxt制作服务器端渲染造成的找不到window和document的问题，是由于有些插件是获取window和document的，在vue中是没有window和document的，所以官方给出了解决方案，
解决方案1：（局部使用（局部获取浏览器的token） 使用插件的时候不要使用，）
```js
if (process.client) {
  require('external_library')
}
```
解决方案2：（使用插件的时候全局注册一下）
注册插件：plugins文件夹内 新建js文件 - 例子：
```js
import Vue from 'vue'
import VueQuillEditor from 'vue-quill-editor'
Vue.use(VueQuillEditor)
```
全局引用：nuxt.config.js配置 ssr:false  服务器端加载
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
 :before-upload="beforeUpload" - 建议使用后这个返回一个false （官网介绍上传文件之前的钩子，参数为上传的文件，若返回 false 或者返回 Promise 且被 reject，则停止上传。类型函数 function(file){}:可取到上传的url 下面代码有介绍）
```html
<template>
  <div style="height: 500px">
    <!-- 图片上传组件
        before-upload：禁止自动上传
        show-file-list是否显示上传列表
        其余属性能加就加     
-->
    <el-upload
      :before-upload="beforeUpload"
      :show-file-list="false"
      accept="image/*"
      action=""
      class="avatar-uploader"
      hidden
      name="file"
      type='file'>
      <el-button size="small" type="primary">点击上传图片 到 文本编辑器</el-button>
    </el-upload>
    <!-- 编辑器组件 @change="onEditorChange($event)" 给父组件发出信息 :options="editorOption" - 传数据给这个组件 -->
    <quill-editor
      style="height: 400px"
      :options="editorOption"
      @change="onEditorChange($event)"
      class="editor"
      ref="myQuillEditor"
      v-model="content">
    </quill-editor>
    <!-- 图片裁剪组件-->
    <el-dialog :visible.sync="isShowCropper" top="5vh">
      <VueCropper
        :autoCrop="option.autoCrop"
        :autoCropHeight="option.autoCropHeight"
        :autoCropWidth="option.autoCropWidth"
        :canScale="option.canScale"
        :fixed="option.fixed"
        :fixedNumber="option.fixedNumber"
        :img="option.img"
        :info="option.info"
        :outputSize="option.outputSize"
        :outputType="option.outputType"
        ref="cropper"
        style="height:600px;margin:20px 0"
      >
      </VueCropper>
      <br/>

      <el-button @click="onCubeImg" type="primary">生成图片</el-button>
      <el-button @click="isShowCropper = false">取消</el-button>
    </el-dialog>
  </div>
</template>
<script>
  // 富文本工具栏配置
  const toolbarOptions = [
    ['bold', 'italic', 'underline', 'strike'], // 加粗 斜体 下划线 删除线
    ['blockquote', 'code-block'], // 引用  代码块
    [{ header: 1 }, { header: 2 }], // 1、2 级标题
    [{ list: 'ordered' }, { list: 'bullet' }], // 有序、无序列表
    [{ script: 'sub' }, { script: 'super' }], // 上标/下标
    [{ indent: '-1' }, { indent: '+1' }], // 缩进
    // [{'direction': 'rtl'}],                         // 文本方向
    [{ size: ['small', false, 'large', 'huge'] }], // 字体大小
    [{ header: [1, 2, 3, 4, 5, 6, false] }], // 标题
    [{ color: [] }, { background: [] }], // 字体颜色、字体背景颜色
    [{ font: [] }], // 字体种类
    [{ align: [] }], // 对齐方式
    ['clean'], // 清除文本格式
    ['link', 'image', 'video'] // 链接、图片、视频
  ]
  //导入的css - 必须 - 最好组件单独导入，不然会乱样式
  import 'quill/dist/quill.core.css'
  import 'quill/dist/quill.snow.css'
  import 'quill/dist/quill.bubble.css'
  //导入上传图片的api
  import { uImage } from '@/api/uploadImage.js'
  export default {
    name: 'textEditor',
    components: {},
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
          theme: 'snow', // or 'bubble'
          placeholder: '请输入您想输入的内容',
          modules: {
            toolbar: {
              container: toolbarOptions,
              // 和上传按钮进行绑定
              handlers: {
                image: function(value) {
                  console.log(value)
                  if (value) {
                    // 触发input框选择图片文件 = 这个位置最好在外面设置一个id之后修改这个的值 #id
                    document.querySelector('#id input').click()
                  } else {
                    this.quill.format('image', false)
                  }
                }
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
          canScale: true,                 // 图片是否允许滚轮缩放
          autoCrop: true,                  // 是否默认生成截图框
          autoCropWidth: 150,              // 默认生成截图框宽度
          autoCropHeight: 150,             // 默认生成截图框高度
          fixed: true,                    // 是否开启截图框宽高固定比例
          fixedNumber: [4, 4]             // 截图框的宽高比例 - 可以使用父组件传来的值 props
        },
        isShowCropper: false,
        isClient: true,
        width: '150px',
        height: '150px'
      }
    },
    methods: {
      // 上传切图前调用
      beforeUpload(file) {
        /**
         * URL.createObjectURL(file)生成本地的url
         * 返回false终止了自动上传
         * */
        this.option.img = URL.createObjectURL(file)
        this.option.autoCropWidth = this.width
        this.option.autoCropHeight = this.height
        this.isShowCropper = true
        return false
      },
      // 确定裁剪图片
      onCubeImg() {
        // 获取cropper的截图的base64 data == base64
        this.$refs.cropper.getCropData(data => {
          //截取bas64 截取base64的格式 和 图片的后缀名
          let baseSplit= data.split(',')
          let format = baseSplit[0].split('/')[1].split(';')[0]
          //获取base64格式的信息
          let base = ''
          if (process.client) {
            base = window.atob(baseSplit[1])
          }
          //转格式：base64转图片
          /**
           *格式为：File
           * lastModified: 1593134876792
           * lastModifiedDate: Fri Jun 26 2020 09:27:56 GMT+0800 (中国标准时间) {}
           * name: "img.png"
           * size: 15407
           * type: "image/png"
           * webkitRelativePath: ""
           * */
          let index = base.length
          let u8arr = new Uint8Array(index)
          while (index--) {
            u8arr[index] = base.charCodeAt(index)
          }
          let blods = new File([u8arr],'img.' + format,{type: 'image/' + format })
          /**
           * 建立一个formData表单，之后把上面的base64转换后的格式使用append传递给FormData
           * */
          let fromData = new FormData()
          fromData.append('file',blods)
          this.$notify({
            title: '成功',
            message: '正在上传中',
            type: 'success'
          });
          //发出网络请求
          uImage(fromData).then(res => {
            this.$notify({
              title: '成功',
              message: '图片上传成功',
              type: 'success'
            });
            // 获取富文本组件实例
            let quill = this.$refs.myQuillEditor.quill
            if(res.code === 200){
              // 获取光标所在位置
              /**
               *  quill.insertEmbed(length, 'image', res.data.url)
               *  图片显示的位置，根据length后的位置显示
               * */
              if (quill.getSelection() && quill.getSelection().index) {
                let length = quill.getSelection().index
                quill.insertEmbed(length, 'image', res.data.url)
              } else {
                let length = 0
                quill.insertEmbed(length, 'image', res.data.url)
              }
              // 调整光标到最后
              quill.setSelection(length + 1)
            }else {
              console.log(res.data.code)
              this.$notify({
                title: '警告',
                message: '图片上传失败',
                type: 'warning'
              });
            }
            this.isShowCropper = false
            // 先将显示图片地址清空，防止重复显示
            this.option.img = ''
          })
        })
      },
      onEditorChange() {
        console.log(this.content)
        //富文本内容改变事件 发送给父级元素 @change事件
        this.$emit('change', this.content)
      }
    },
    mounted() {
      setTimeout(() => {
        this.content = this.value
      }, 500)
    }
  }
</script>
<style lang="scss">
</style>
```


