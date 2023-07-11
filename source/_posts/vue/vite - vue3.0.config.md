---
uuid: 81fcd500-a4d5-a890-0cf0-46fd315d66f1
title: vite - vue3.0 vite.config
date: 2023-06-08 14:23:11
tags: vue vite.config vite
category: vue
---
## vite.config.js
vite 安装后，根目录下创建配置文件vite.config.js，默认加载此配置文件
可通过工具函数定义，获得类型提示。defineConfig
异步获取配置信息数据
解决热更新问题 - import { resolve } from "path";
之后resolve:{alias: {"@": resolve(__dirname, "./src")}}
```js
import { defineConfig,loadEnv } from 'vite'
import { resolve } from "path";// 全局热更新需要在这个
import vue from '@vitejs/plugin-vue'
// https://vitejs.dev/config/

export default defineConfig(async ({ command, mode, ssrBuild }) => {
  console.log(mode) // 环境判断
  const env = loadEnv(mode, process.cwd(), '')
  console.log(env.APP_ENV) // 是否是服务器端渲染
  if (command === 'serve') {
  } else {
  }
  return {
    server:{hmr:true},
    resolve: {
      // 设置文件目录别名
      // 之后在这里配置
      alias: {
        "@": resolve(__dirname, "./src"),
      },
      extensions: [".js"],
    },
    plugins:[vue()],
  }
})

```

## vite.config.js配置
resolve**
plugins:[vue()]**

- root : '/'
  项目根目录，index.html 所在的目录
- base : ''
  生产或开发环境下的基础路径
- plugins: [] :
  需要用到的插件数组
- publicDir:''
  静态资源服务目录地址
- cacheDir:''
  存储缓存文件的目录地址
- resolve:{alias:{"@":"./src"},extensions:['.js']}
  alias:设置文件目录别名 extensions:省略后缀
- css:{modules:{},preprocessorOptions:{less:{}}}
  preprocessorOptions:指定less预处理的配置项
- esbuild:{}
  esbuild 选项转换配置
- assetsInclude:{}
  静态资源处理
- server:{host:'',port:''}
  开发服务器选项
- build:{ outDir:'',assetsDir:'',cssCodeSplit:true,sourcemap:'inline'rollupOptions:{}}
  构建配置项  outDir:指定输出目录；assetsDir:指定静态资源存放目录；cssCodeSplit:启用、禁用css代码拆分；.
  sourcemap：构建是否生成source map文件；rollup: 配置打包项
- optimizeDeps:{entries:'',exclude:[]}
  依赖优化配置项  依赖构建入口  排除不需要构建的依赖项
    

```js
// 类型提示
import {defineConfig} from 'vite'

// config
export default defineConfig(({command,mode})=>{
  /**
   * command - 命令模式
   * mode - 生产、开发模式
   */
  
  return {
    // 项目根目录，index.html 所在的目录
    root:'/',
    // 生产或开发环境下的基础路径
    base:'/hboot/',
    // 需要用到的插件数组
    plugins: [],
    // 静态资源服务目录地址
    publicDir:'',
    // 存储缓存文件的目录地址
    cacheDir:'',
    // 
    resolve:{
      // 设置文件目录别名
      alias:{
        "@":"/src"
      },
      //
      extensions:['.js']
    },
    //
    css:{
      // postcss-modules 行为配置
      modules:{
        // ...
      },
      // 传递给css预处理器的配置项
      preprocessorOptions:{
        // 指定less预处理的配置项
        less:{
          // ...
        }
      }
    },
    // esbuild 选项转换配置
    esbuild:{
      // ...
      // 在react组件中无需导入react
      jsxInject: `import React from 'react'`,
      // vue 使用jsx
      jsxFactory:'h',
      jsxFragment:'Fragment'
    },
    // 静态资源处理
    assetsInclude: '',
    // 开发服务器选项
    server:{
      // ...
      host:'',
      port:''
    },
    // 构建配置项
    build:{
      // ...
      // 指定输出目录
      outDir: '',
      // 指定静态资源存放目录
      assetsDir:"",
      // 启用、禁用css代码拆分
      cssCodeSplit:true,
      // 构建是否生成source map文件
      sourcemap:'inline',
      // rollup 配置打包项
      rollupOptions:{
        // ...
      }
    },
    // 依赖优化配置项
    optimizeDeps:{
      // 依赖构建入口
      entries:'',
      // 排除不需要构建的依赖项
      exclude:[],
    }
  }
})
```



