<template>
  <div style="width: 100%; height: 100vh; background-color: darkslateblue; overflow: hidden">
    <div style="width: 300px; margin: 150px auto">
      <div style="color: #cccccc; font-size: 30px; text-align: center; padding: 30px 0">欢迎登录</div>
        <el-form ref="form" :model="form" size="normal" :rules="rules">
          <el-form-item prop="username">
            <el-input v-model="form.username" placeholder="请输入用户名">
              <template #prefix>
                <el-icon><UserFilled /></el-icon>
              </template>
            </el-input>
          </el-form-item>
          <el-form-item prop="password">
            <el-input
                v-model="form.password"
                type="password"
                placeholder="请输入密码"
                show-password
            >
              <template #prefix>
                <el-icon><Lock /></el-icon>
              </template>
            </el-input>
          </el-form-item>
          <el-button style="width: 100%" type="primary" @click="login">登 录</el-button>
        </el-form>
    </div>
  </div>
</template>

<script>

import {Lock, Search, UserFilled} from "@element-plus/icons-vue";
import request from "@/utils/request";

export default {
  name: "Login",
  components: {
    Lock,
    Search,
    UserFilled
  },

  data() {
    return {
      form: {
      },
      rules: {
        username: [
          {required: true, message: '请输入用户名', trigger: 'blur'},
        ],
        password: [
          {required: true, message: '请输入密码', trigger: 'blur'}
        ]
      }
    }
  },
  methods:{
    login(){
      this.$refs['form'].validate((valid) => {
        if(valid){
          request.post("/user/login",this.form).then(res =>{
            if(res.code === '0'){
              this.$message({
                type: "success",
                message: "登录成功"
              })
              this.$router.push("/")  //登录成功之后 进行页面的跳转 , 跳转到主页
            }else {
              this.$message({
                type: "error",
                message: res.msg
              })
            }
          })
        }

      })
    }
  }
}

</script>

<style scoped>

</style>