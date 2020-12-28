<template>
  <img alt="Vue logo" src="./assets/logo.png">
  <div class="validate-form-container ">
    <ValidateForm @submit="submitForm" class="w-400 m-auto">
      <div class="form-group row">
        <label class="col-3 col-form-label">邮箱地址</label>
        <div class="col-8">
            <ValidateInput  v-model="formData.email" :rules="emailRules" placeholder="请输入正确的电子邮箱地址"/>
        </div>
      </div>
      <div class="form-group row">
        <label class="col-3 col-form-label">用户名</label>
        <div class="col-8">
            <ValidateInput  v-model="formData.name" :rules="nameRules" placeholder="请输入用户名"/>
        </div>
      </div>
    </ValidateForm>
  </div>
</template>

<script lang="ts">
import { defineComponent, reactive, ref } from 'vue'
import ValidateInput, { RulesProp } from './components/ValidateInput.vue'
import ValidateForm from './components/ValidateForm.vue'
import 'bootstrap/dist/css/bootstrap.min.css'
export default defineComponent({
  name: 'App',
  components: {
    ValidateInput,
    ValidateForm
  },
  setup () {
    const formData = reactive({
      email: '',
      name: ''
    })
    const emailRules: RulesProp = [
      { type: 'required', message: '电子邮箱不能为空' },
      { type: 'email', message: '请输入正确的电子邮箱格式' }
    ]
    const nameRules: RulesProp = [
      { type: 'required', message: '请输入用户名' }
    ]
    const submitForm = function (result: boolean) {
      console.log(result)
    }
    return {
      formData,
      emailRules,
      submitForm,
      nameRules
    }
  }
})
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.w-400{
  width: 400px;
}
</style>
