/* eslint-disable no-unreachable */
<template>
    <div class="login_container">
        <div class="login_box">
            <!-- 头像区域 -->
            <div class="avatar_box">
                <img src="../assets/logo.png" alt="" />
            </div>
            <!-- 登录表单区域 -->
            <el-form
                ref="loginFormRef"
                :model="loginForm"
                :rules="loginRules"
                label-width="0px"
                class="login_form"
            >
                <!-- 用户名 -->
                <el-form-item prop="username">
                    <el-input
                        v-model="loginForm.username"
                        prefix-icon="iconfont icon-users"
                    ></el-input>
                </el-form-item>
                <!-- 密码 -->
                <el-form-item prop="password">
                    <el-input
                        v-model="loginForm.password"
                        type="password"
                        prefix-icon="iconfont icon-3702mima"
                    ></el-input>
                </el-form-item>
                <!-- 按钮区域 -->
                <el-form-item class="btns">
                    <el-button type="primary" @click="login">登录</el-button>
                    <el-button type="info" @click="resetLoginForm"
                        >重置</el-button
                    >
                </el-form-item>
            </el-form>
        </div>
    </div>
</template>

<script>
    export default {
        data () {
            return {
                loginForm: {
                    username: 'admin',
                    password: '123456'
                },
                loginRules: {
                    username: [
                        {
                            required: true,
                            message: '请输入用户名',
                            trigger: 'blur'
                        },
                        {
                            min: 5,
                            max: 10,
                            message: '长度在 5 到 10 个字符',
                            trigger: 'blur'
                        }
                    ],
                    password: [
                        {
                            required: true,
                            message: '请输入登录密码',
                            trigger: 'blur'
                        },
                        {
                            min: 6,
                            max: 12,
                            message: '长度在 6 到 12 个字符',
                            trigger: 'blur'
                        }
                    ]
                }
            }
        },
        methods: {
            // 点击重置按钮。充值登陆表单
            resetLoginForm () {
                this.$refs.loginFormRef.resetFields()
            },
            // 点击提交按钮，提交表单
            login () {
                this.$refs.loginFormRef.validate(async valid => {
                    if (!valid) return
                    const { data: res } = await this.$http.post(
                        'login',
                        this.loginForm
                    )
                    if (res.meta.status !== 200) return this.$message.error('登陆失败!')
                    // console.log(res)
                    window.sessionStorage.setItem('token', res.data.token)
                    this.$router.push('/home')
                    return this.$message.success('登陆成功!')
                })
            }
        }
    }
</script>

<style lang="less" scoped>
.login_container {
    background-color: #2b4b6b;
    height: 100%;
}
.login_box {
    width: 450px;
    height: 300px;
    background-color: #ffffff;
    border-radius: 3px;
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
    .avatar_box {
        height: 100px;
        width: 300px;
        border: 1px solid #eeeeee;
        border-radius: 50%;
        padding: 10px;
        box-shadow: 0 0 10px #dddddd;
        position: absolute;
        left: 50%;
        transform: translate(-50%, -50%);
        img {
            width: 100%;
            height: 100%;
            border-radius: 50%;
            background-color: darkblue;
        }
    }
    .btns {
        display: flex;
        justify-content: flex-end;
    }
    .login_form {
        position: absolute;
        bottom: 10px;
        width: 100%;
        padding: 0 20px;
        box-sizing: border-box;
    }
}
</style>
