---
uuid: c26759f6-cb77-ee98-f55e-6f0b5d2bf96c
title: nuxt单个裁切图片和多个裁切
date: 2020-07-05 09:10:29
tags: nuxt单个裁切图片和多个裁切
category: Nuxt
---
安装方法安装富文本图片裁切：npm install -d vue-cropper 
vue-cropper：https://github.com/xyxiao001/vue-cropper
之后直接可以使用了，我是用的element上传的方法，单个图片裁切和多个图片裁切， - 问题：裁切之后鼠标不能滚动的问题，使用v-if在弹出框上解决一下看看
<!-- more -->
直接上代码
cropperPic：同一个页面使用多个上传，需要添加传入内容：url：‘图片地址’，name：‘唯一标识’ - 单个上传的时候直接{url:"图片地址"}
uploadImage：父组件接收子组件传来的参数
父组件使用： <vue-cropper-image :cropper-pic="img" @uploadImage="uploadImage" v-if="isShowCropper">
```js
//传回来的值命名img 把img的地址传给相应的人
uploadImage(img) {
        if (img.name === 'logo') {
//一张图片的时候传给logo
          this.ruleForm.logo = img.url
        }
        if (img.name === 'banner') {
//多张图片直接push进去
          let url = img.url
          this.ruleForm.banners.push(url)
        }
        this.isShowCropper = false
      },
//多张图片删除，我自己写了一个循环遍历 - <img v-for> 之后点击那个删除那个
 bannerDel(index) {
        this.ruleForm.banners.splice(index, 1)
      }
    },
```

```html
<template>
  <div>
<!-- :img="imageUrl" 在计算属性中return出来了 -->
    <VueCropper
      :autoCrop="option.autoCrop"
      :autoCropHeight="option.autoCropHeight"
      :autoCropWidth="option.autoCropWidth"
      :canScale="option.canScale"
      :fixed="option.fixed"
      :fixedNumber="option.fixedNumber"
      :img="imageUrl"
      :info="option.info"
      :outputSize="option.outputSize"
      :outputType="option.outputType"
      ref="cropper"
      style="height:600px;margin:20px 0"
    >
    </VueCropper>
    <br/>
    <el-button @click="onCubeImg" type="primary">生成图片</el-button>
    <el-button @click="onCubeImgOff" type="primary">关闭</el-button>
  </div>
</template>

<script>
//网络请求
  import { uImage } from '@/api/uploadImage.js'

  export default {
    name: 'vueCropperImage',
    props: {
//接收传入的url 是一个对象，因为需要判断是谁传来的 ： url：图片地址，name：‘标识’ 同一页面使用多个
      cropperPic: {
        type: Object,
        default() {
          return {}
        }
      },
      //传入来的比例，1：1正方形类似
      fixedNum:{
        type:Object,
        default() {
          return {
            a:2,
            b:1
          };
        }
      }
    },
    data() {
      return {
        option: {
          img:'' ,                        // 裁剪图片的地址
          info: true,                      // 裁剪框的大小信息
          outputSize: 1,                   // 裁剪生成图片的质量
          outputType: 'png',               // 裁剪生成图片的格式
          canScale: true,                 // 图片是否允许滚轮缩放
          autoCrop: true,                  // 是否默认生成截图框
          fixed: true,                    // 是否开启截图框宽高固定比例
          fixedNumber: [this.fixedNum.a, this.fixedNum.b] // 截图框的宽高比例
        },
      }
    },
    computed:{
      imageUrl(){
        return this.cropperPic.url
      },
    },
    methods: {
      // 关闭 点击关闭进行关闭
      onCubeImgOff(){
        this.$emit('onCubeImgOff')
      },
      //上传
      onCubeImg() {
        this.$refs.cropper.getCropData(data => {
          //截取bas64 截取base64的格式 和 图片的后缀名
          let baseSplit = data.split(',')
          let format = baseSplit[0].split('/')[1].split(';')[0]
          //获取base64格式的信息
          let base = ''
          if (process.client) {
            base = window.atob(baseSplit[1])
          }
          //转格式：base64转图片
          let index = base.length
          let u8arr = new Uint8Array(index)
          while (index--) {
            u8arr[index] = base.charCodeAt(index)
          }
          let blods = new File([u8arr], 'img.' + format, { type: 'image/' + format })
          let fromData = new FormData()
          fromData.append('file', blods)


          this.$notify({
            title: '成功',
            message: '正在上传中',
            type: 'success'
          })
          uImage(fromData).then(res => {
            console.log(res)
            if (res.code === 200) {
              this.$notify({
                title: '成功',
                message: '图片上传成功',
                type: 'success'
              })
              let param = {
                url: res.data.url,
                name: this.cropperPic.name
              }
              this.$emit('uploadImage', param)
            } else {
              this.$notify({
                title: '警告',
                message: '图片上传失败',
                type: 'warning'
              })
            }
          })
        })
      }
    },

  }
</script>

<style scoped>

</style>

```