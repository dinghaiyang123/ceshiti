<template>
  <span class="header-span">欢迎来到VueAdmin管理系统</span>
  <div id="login-container">
    <a-form
      :label-col="{ span: 4 }"
      :model="formState"
      :wrapper-col="{ span: 20 }"
      autocomplete="off"
      name="basic"
      @finish="onFinish"
      @finishFailed="onFinishFailed"
    >
      <a-form-item
        :rules="[{ required: true, message: '请输入用户名!' }]"
        label="用户名"
        name="username"
      >
        <a-input v-model:value="formState.username"/>
      </a-form-item>

      <a-form-item
        :rules="[{ required: true, message: '请输入密码!' }]"
        label="密    码"
        name="password"
      >
        <a-input-password v-model:value="formState.password"/>
      </a-form-item>
      <a-form-item
        :rules="[{ required: true, message: '请输入验证码!' }]"
        class="code_box"
        label="验证码"
        name="code"
      >
        <a-input v-model:value="formState.code" class="codeImg" style="width: 200px"/>
        <img :src="captchaImg" alt="" @click="getCodeUrl">
      </a-form-item>
      <a-form-item :wrapper-col="{ offset: 11, span: 13 }">
        <a-button class="register" html-type="submit" type="primary" @keydown.enter="onFinish">登录</a-button>
      </a-form-item>
    </a-form>
  </div>
</template>

<script setup>
import { reactive, ref } from 'vue'
import { apiLogin, getCaptcha } from '@/api/login'
import { useRouter } from 'vue-router'

const formState = reactive({
  username: 'test',
  password: '1234567',
  code: null,
  token: null
})
const router = useRouter()
const captchaImg = ref('')
const getCodeUrl = async () => {
  const res = await getCaptcha()
  console.log(res)
  captchaImg.value = res.captchaImg
  formState.token = res.token
}
getCodeUrl()
const onFinish = async () => {
  try {
    console.log(formState)
    await apiLogin(formState)
    await router.push('/')
  } catch (error) {

  }
}

const onFinishFailed = errorInfo => {
  console.log('Failed:', errorInfo)
}
</script>
<style lang="scss" scoped>
.header-span{
  display: inline-block;
  color: #000;
  font-weight: 800;
  font-size: 25px;
  // margin-left: 510px;
  // margin-top: 80px;
  margin:20px auto;
}
#login-container {
  width: 520px;
  height: 300px;
  margin: 0 auto;
  border-radius: 5px;
  background: -webkit-gradient(linear, 0 0, 0 40%, from(#2a9fd6c4), to(rgba(14, 173, 184, 0.24)));
  padding-top: 30px;
  padding-right: 30px;
  .register{
    border: none;
    color: #000;
    font-weight: 500;
    font-size: 16px;
    background: -webkit-gradient(linear, 0 0, 0 40%, from(#00000077), to(rgba(255, 255, 255, 0.24)));
  }
}

.codeImg {
  margin-right: 10px;
}
</style>
