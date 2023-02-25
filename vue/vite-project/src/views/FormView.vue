<script setup>
import {onMounted, reactive, ref} from 'vue'
import Schema from 'async-validator'

const form = reactive({
  phone: '',
  password: '',
  remember: false
})
const errorMessage = ref([])
const rules = {
  phone: [
    { required: true, message: '请输入手机号' },
    { required: true, pattern: /^(?:(?:\+|00)86)?1[3-9]\d{9}$/, message: '手机号不合法' },
  ],
  password: [
    { required: true, message: '请输入密码' },
    { required: true,min:6, max:18, message: '密码必须6-18个字符之间' },
  ]
}
const validator = new Schema(rules)
const getMessage = (errors,field)=>{
  return errors.find(item=>item.field === field) || {}
}
onMounted(async ()=>{
  const rememberToken = localStorage.getItem('rememberToken')
  if(rememberToken){
    await autoLogin();
  }
})
// 记住密码登录
const autoLogin = ()=>{
  return Promise.resolve('success')
}
// 模拟登录
const login = (form)=>{
  const res = {
    token: 'abcdefg',
    userInfo: {}
  }
  if(form.remember){
    res.rememberToken = 'aaccasdffgh'
  }
  return Promise.resolve(res)
}
const handleSubmit = ()=>{
  console.log(form)
  validator.validate(form, async (errors, fields) => {
    if(errors){
      errorMessage.value = errors
      return;
    }
    const res = await login(form)
    // “记住密码”，这个功能最终是将相关信息保存在localStorage中，从而实现自动登录的
    localStorage.setItem('rememberToken', res.rememberToken)
    console.log('res',res)
  })

}
</script>

<template>
  <section>
    <div class="form-wrap">
      <h1>登录表单</h1>
      <form>
        <div class="form-control">
          <label>手机号</label>
          <input type="text" v-model="form.phone" placeholder="请输入手机号">
          <div class="help-block">{{getMessage(errorMessage, 'phone').message}}</div>
        </div>

        <div class="form-control">
          <label>密码</label>
          <input type="password" v-model="form.password" placeholder="请输入密码">
          <div class="help-block">{{getMessage(errorMessage, 'password').message}}</div>
        </div>
        <div class="pb-2">
          <label>
            <input type="checkbox" v-model="form.remember">记住密码
          </label>
        </div>
        <button type="button" class="btn" @click="handleSubmit">登录</button>
      </form>
    </div>
  </section>
</template>

<style scoped>
* {
  box-sizing: border-box;
}

section {
  background-color: steelblue;
  color: #fff;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100vh;
  overflow: hidden;
  margin: 0;
}

.form-wrap {
  background-color: rgba(0, 0, 0, 0.4);
  padding: 20px 40px;
  border-radius: 5px;
}

.form-wrap h1 {
  text-align: center;
  margin-bottom: 30px;
}

.form-wrap a {
  text-decoration: none;
  color: lightblue;
}

.btn {
  @apply bg-sky-600;
  cursor: pointer;
  display: inline-block;
  width: 100%;
  padding: 15px;
  font-family: inherit;
  font-size: 16px;
  border: 0;
  border-radius: 5px;
}

.btn:focus {
  outline: 0;
}

.btn:active {
  transform: scale(0.98);
}

.text {
  margin-top: 30px;
}

.form-control {
  position: relative;
  margin: 20px 0 40px;
  width: 300px;
}

.form-control input {
  background-color: transparent;
  border: 0;
  border-bottom: 2px #fff solid;
  display: block;
  width: 100%;
  padding: 15px 0 15px 50px;
  font-size: 18px;
  color: #fff;
}

.form-control input:focus,
.form-control input:valid {
  outline: 0;
  border-bottom-color: lightblue;
}

.form-control label {
  position: absolute;
  top: 15px;
  left: 0;
  pointer-events: none;
}

.form-control label span {
  display: inline-block;
  font-size: 18px;
  min-width: 5px;
  transition: 0.3s cubic-bezier(0.68, -0.55, 0.265, 1.55);
}

.form-control input:focus + label span,
.form-control input:valid + label span {
  color: lightblue;
  transform: translateY(-30px);
}
.help-block{
  position: absolute;
  top:100%;
  color: crimson;
}
</style>
