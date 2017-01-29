<template>
  <div class="login_form_wrapper">
    <div class="login_form_wrapper_cell">
      <h3>登录管理后台</h3>
      <el-form :model="loginForm" :rules="rules" ref="loginForm" label-width="60px">
        <el-form-item label="用户名" prop="username">
          <el-input type="text" v-model="loginForm.username" auto-complete="off"></el-input>
        </el-form-item>
        <el-form-item label="密码" prop="password">
          <el-input type="password" v-model="loginForm.password" auto-complete="off"></el-input>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="handleSubmit">提交</el-button>
          <el-button @click="handleReset">重置</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>
<style scoped>
  .login_form_wrapper {
    width: 100%;
    height: 100%;
    display:table;
    background-color: #000000;
  }
  .login_form_wrapper_cell {
    display:table-cell;
    text-align: center;
    vertical-align:middle;
    color: white;
  }
  .login_form_wrapper form {
    display: inline-block;
    width: 500px;
    margin: auto;
    background-color: #dddddd;
    border-radius: 5px;
    padding: 50px;
    margin-bottom: 200px;
  }
</style>
<script>
  export default {
    data () {
      var validatesUsername = (rule, value, callback) => {
        if (value === '') {
          callback(new Error('请输入用户名'))
        } else {
          callback()
        }
      }
      var validatePassword = (rule, value, callback) => {
        if (value === '') {
          callback(new Error('请输入密码'))
        } else {
          callback()
        }
      }
      return {
        loginForm: {
          username: '',
          password: ''
        },
        rules: {
          username: [
            { validator: validatesUsername, trigger: 'blur' }
          ],
          password: [
            { validator: validatePassword, trigger: 'blur' }
          ]
        }
      }
    },
    methods: {
      handleReset () {
        this.$refs.loginForm.resetFields()
      },
      handleSubmit (ev) {
        this.$refs.loginForm.validate((valid) => {
          if (valid) {
            this.checkAccount()
          } else {
            console.log('error submit!!')
            return false
          }
        })
      },
      checkAccount () {
        // TODO:查询服务器
        let vm = this

        vm.InterfaceLogin(vm.loginForm.username, vm.loginForm.password, function (administrator, token) {
          console.log(administrator, token)
          vm.$store.commit('LOGIN_SUCCESS', {
            administrator,
            token
          })
          vm.$router.push('/workbench/dashboard')
        })
      },
      // 登录接口
      InterfaceLogin (username, password, callback) {
        switch (this.$store.state.config.ApiAdapter) {
          case 'apigility':
            return this.InterfaceLogin_Apigility(username, password, callback)
        }
      },
      // 登录接口的实现之一
      InterfaceLogin_Apigility (username, password, callback) {
        let vm = this

        this.$http.get('admin/administrator', {
          params: {
            name: username
          }
        }).then((response) => {
          response.json().then(function (data) {
            if (response.ok && data.total_items > 0) {
              let administrator = data._embedded.administrator[0]
              vm.$http.post('oauth', {
                grant_type: 'password',
                username: data._embedded.administrator[0].user.id,
                password: password
              }, {
                headers: {
                  Authorization: vm.$store.state.login.authorization
                }
              }).then((response) => {
                callback(administrator, response.body)
              }, (response) => {
                window.alert('用户名或密码不正确！')
              })
            } else {
              window.alert('管理员帐号不存在')
            }
          })
        })
      }
    },
    created: function () {
      if (this.$store.state.login.status) {
        this.$store.commit('LOGOUT_SUCCESS')
      }
    }
  }
</script>
