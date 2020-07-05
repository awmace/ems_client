<template>
    <div id="wrap">
        <div id="top_content">
            <div id="header">
                <div id="rightheader">
                    <p>
                        2009/11/20
                        <br/>
                    </p>
                </div>
                <div id="topheader">
                    <h1 id="title">
                        <a href="#">main</a>
                    </h1>
                </div>
                <div id="navigation">
                </div>
            </div>
            <div id="content">
                <p id="whereami">
                </p>
                <h1>
                    注册
                </h1>
                <table cellpadding="0" cellspacing="0" border="0"
                       class="form_table">
                    <tr>
                        <td valign="middle" align="right">
                            用户名:
                        </td>
                        <td valign="middle" align="left">
                            <input type="text" class="inputgri" name="username" v-model="username"/>
                        </td>
                    </tr>
                    <tr>
                        <td valign="middle" align="right">
                            真实姓名:
                        </td>
                        <td valign="middle" align="left">
                            <input type="text" class="inputgri" name="name" v-model="tname"/>
                        </td>
                    </tr>
                    <tr>
                        <td valign="middle" align="right">
                            密码:
                        </td>
                        <td valign="middle" align="left">
                            <input type="password" class="inputgri" name="pwd" v-model="password"/>
                        </td>
                    </tr>
                    <tr>
                        <td valign="middle" align="right">
                            确认密码:
                        </td>
                        <td valign="middle" align="left">
                            <input type="password" class="inputgri" name="pwd" v-model="passwords"/>
                        </td>
                    </tr>
                    <tr>
                        <td valign="middle" align="right">
                            性别:
                        </td>
                        <td valign="middle" align="left">
                            男
                            <input type="radio" class="inputgri" name="sex" value="m" checked="checked" @click="gender=1"/>
                            女
                            <input type="radio" class="inputgri" name="sex" value="f" @click="gender=0"/>
                        </td>
                    </tr>

                </table>
                <p>
<!--                    <input type="submit" class="button" value=""/>-->
                    <el-button type="success" @click="user_register">提交注册</el-button>
                </p>
            </div>
        </div>
        <div id="footer">
            <div id="footer_bg">
                ABC@126.com
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        name: "Register",
        data(){
            return {
                username:'',
                tname:'',
                password:'',
                passwords:'',
                gender:0,
            }
        },
        methods:{
            //向后端提交注册请求
            user_register(){
                console.log('111')
                this.$axios({
                    url:"http://127.0.0.1:8000/api/user/",
                    method:'post',
                    data: {
                        username:this.username,
                        password:this.password,
                        tname:this.tname,
                        gender:this.gender,
                        passwords:this.passwords,
                    }
                }).then(res=>{
                    //如果注册成功，跳转到登录页面
                    if (res.data['message']){
                        this.$message('恭喜您注册成功了')
                        this.$router.push('/login');
                    }
                    else if (res.data['status']==201){
                        this.$message('两次密码不一致')
                    }
                }).catch(error=>{
                    this.$message.error('温馨提示:您注册失败了')
                })
            }
        }
    }
</script>

<style scoped>

</style>
