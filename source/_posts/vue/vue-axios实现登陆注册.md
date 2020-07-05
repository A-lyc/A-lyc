---
uuid: 2cbc4fd5-d5e6-2d72-8a22-f8d83bde367d
title: vue-axios实现登陆注册
date: 2020-05-01 15:15:29
tags: vue 登录注册
category: vue
---
vue的实现等登录注册，使用vuex来管理登录状态，登录的时候就是接收axios成功把用户名等信息保存起来，之后保存到浏览器的方法内（因为刷新的时候要保证登陆状态是存在的，关闭浏览器的时候登陆状态是没有的，退出登录的是时候登录状态没有，）

首先通过axios请求来的数据保存到vuex中去，在mutations中定义几个方法，：
<!-- more -->
```js
//初始化用户信息
    initUser(state){
      let user = window.sessionStorage.getItem('user')
      if(user){
        state.user = JSON.parse(user)
        state.token = state.user.token
      }
      console.log('初始化调用成功')
    },
    //登录
    login(state,user){
      //保存登录的状态
      state.user = user;
      state.token = user.token
      //存储到本地当中
      window.sessionStorage.setItem('user', JSON.stringify(state.user))
      window.sessionStorage.setItem('token', JSON.stringify(state.token))
    },
    //退出登录
    logout(state){
      //清除状态
      state.user = {}
      state.token = ''
      //清除本地存储
      window.sessionStorage.clear()
    }
```
浏览器的sessionStorage方法刷新之后不会被清理（暂时理解）：
window.sessionStorage.getItem => 读取，获取
window.sessionStorage.setItem => 写入
window.sessionStorage.clear => 清除

登录使用：
```js
   submit() {
      this.$refs.ruleForm.validate(e => {//框架表单验证
        if (!e) return;
        // 提交表单
        this.axios
          .post("url", this.form)//post请求传入的用户名和信息给后端验证之后返回数据
          .then(res => {
            //请求成功打印的数据
            console.log(res);
            //存储到本地 //成功之后存储vuex中方法为login
            this.$store.commit("login", res.data.data);
            //成功提示
            this.$message("登陆成功");
            //跳转到后台
            this.$router.push({name:'index'})
          })
          .catch(err => {
            if (err.response.data && err.response.data.errorCode) {
              this.$message.error(err.response.data.msg);
            }
          });
      });
    }
```

退出使用vuex
```js
logout() {
      this.axios
        .post(
          "/admin/logout",
          {},
          {
            headers: {
              token: this.user.token
            }
          }
        )
        .then(res => {
						//退出登陆状态和储存
          this.$store.commit("logout");
          //返回登录页
          this.$router.push({ name: "login" });
          console.log(res);
        })
        .catch(err => {
          if (err.response.data && err.response.data.errorCode) {
						this.$message.error(err.response.data.msg);
						//退出登陆状态和储存
            this.$store.commit("logout");
            //返回登录页
            this.$router.push({ name: "login" });
          }
        });
    }
```