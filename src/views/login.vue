<template>
  <el-row type="flex" class="login-form" justify="center" align="middle">
    <el-col :xs="14" :sm="10" :md="8" :lg="8" :xl="6">
      <el-form
        :model="ruleForm"
        :rules="rules"
        ref="ruleForm"
        label-width="100px"
        class="demo-ruleForm"
        label-position='top'
      >
        <el-form-item label="用户名" prop="username">
          <el-input v-model="ruleForm.username"></el-input>
        </el-form-item>
        <el-form-item label="密码" prop="password">
          <el-input v-model="ruleForm.password"></el-input>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="submitForm('ruleForm')">登陆</el-button>
          <el-button @click="resetForm('ruleForm')">重置</el-button>
        </el-form-item>
      </el-form>
    </el-col>
  </el-row>
</template>
<script>
import axios from 'axios'
import { constants } from 'crypto';
   export default {
    data() {
      return {
        ruleForm: {
          username: 'admin',
          password: '123456'
        },
        rules: {
          username: [
            { required: true, message: '请输入用户名', trigger: 'blur' },
            { min: 5, max: 8, message: '长度在 5 到 8个字符', trigger: 'blur' }
          ],
          password: [
           { required: true, message: '请输入密码', trigger: 'blur' },
            { min: 5, max: 12, message: '长度在 5 到 12 个字符', trigger: 'blur' }
          ],
        }
      };
    },
    methods: {
      submitForm(formName) {
        this.$refs[formName].validate((valid) => {
          if (valid) {
            axios({
              url : 'http://localhost:8888/api/private/v1/login',
              method : 'post',
              data : this.ruleForm
              // 解构
            }).then(({data: {data, meta}}) => {
            //  console.log( res)
            console.log(data , meta)
            if(meta.status === 200) {
              localStorage.setItem('token', data.token)
              //编程式导航
              this.$router.push("/home")
            }
            })
          } else {
            return false;
          }
        });
      },
      resetForm(formName) {
        this.$refs[formName].resetFields();
      }
    }
   }
</script>
<style>
.login-form {
  height: 100%;
  background-color: #1e7a72;
}
.demo-ruleForm {
  background-color: #fff;
  padding: 35px 20px;
  min-width: 400px;
  border-radius: 15px;
}
</style>

