<template>
    <div>
        <el-form :rules="rules" ref="loginForm" :model="LoginForm" class="loginContainer">
            <h3 class="loginTitle">系统登录</h3>
            <el-form-item prop="username">
                <el-input type="text" v-model="LoginForm.username" auto-complete="off" placeholder="请输入用户名"></el-input>
            </el-form-item>
            <el-form-item prop="password">
                <el-input type="text" v-model="LoginForm.password" auto-complete="off" placeholder="请输入密码"></el-input>
            </el-form-item>
            <el-checkbox v-model="checked" class="loginRemember"></el-checkbox>
            <el-button type="primary" style="width: 100%" @click="submitLogin">登录</el-button>
        </el-form>
    </div>
</template>

<script>
    import {postKeyValueRequest} from "../util/api";

    export default {
        name: "Login",
        data() {
            return {
                rules: {
                    username: [{ required: true, message: '请输入用户名', trigger: 'blur' }],
                    password: [{ required: true, message: '请输入密码', trigger: 'blur' }]
                },
                LoginForm: {
                    username: 'admin',
                    password: '123'
                },
                checked: true
            }
        },
        methods: {
            submitLogin() {
                this.$refs.loginForm.validate((valid) => {
                    if (valid) {
                        postKeyValueRequest('/doLogin', this.LoginForm).then(resp => {
                            if (resp) {
                                window.sessionStorage.setItem("user", JSON.stringify(resp.obj));
                                this.$router.replace('/home')
                            }
                        })
                    } else {
                        this.$message({
                            showClose: true,
                            message: '请输入所有字段',
                            type: 'error'
                        });
                        return false;
                    }
                });
            }
        }
    }
</script>

<style>
    .loginContainer {
        border-radius: 15px;
        background-clip: padding-box;
        margin: 180px auto;
        width: 350px;
        padding: 15px 15px 15px 15px;
        background: #fff;
        border: 1px solid #eaeaea;
        box-shadow: 0 0 25px #cac6c6;
    }
    .loginTitle {
        margin: 15px auto 40px auto;
        text-align: center;
        color: #505458;
    }
    .loginRemember {
        text-align: left;
        margin: 0px 0px 15px 0px;
    }
</style>