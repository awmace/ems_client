<template>
    <div id="wrap">
        <div id="top_content">
            <div id="header">
                <div id="rightheader">
                    <p>
                        2009/11/20
                        <br/>
                        <a href="javascript:void(0)" @click="clear">安全退出</a>
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
                    欢迎{{username}}登录本系统
                </h1>
                <table class="table">
                    <tr class="table_header">
                        <td>
                            ID
                        </td>
                        <td>
                            Name
                        </td>
                        <td>
                            Photo
                        </td>
                        <td>
                            Salary
                        </td>
                        <td>
                            Age
                        </td>
                        <td>
                            Operation
                        </td>
                    </tr>
                    <tr v-for="(emp,index) in emp_list" :key="emp.id" :class="index%2==0?'row1':'row2'">
                        <td>
                            {{emp.id}}
                        </td>
                        <td>
                            {{emp.emp_name}}
                        </td>
                        <td>
                            <img :src="emp.img" style="height: 60px;">
                        </td>
                        <td>
                            {{emp.salary}}
                        </td>
                        <td>
                            {{emp.age_name}}
                        </td>
                        <td>
                            <router-link :to="'/update/?id='+emp.id">修改员工</router-link>&nbsp;
                            <a href="javascript:void(0)" @click="deleteEmp(emp.id,index)">删除员工</a>
                        </td>
                    </tr>
                </table>
                <p>
<!--                    <input type="button" class="button" value="添加"/>-->
                    <el-button type="success"><router-link to="/add">添加员工</router-link></el-button>
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
        name: "Index",
        created() {
            let user =JSON.parse(sessionStorage.getItem('user'))
            if (user){
                //显示用户名
                this.username=user['username']
            }
            else{
                this.$message.error("您还没有登录，请登录")
                this.$router.push('/login')
            }
            this.findallemp()
        },
        data(){
            return {
                username:'',
                emp_list:[],  //员工列表
            }
        },
        methods:{
            clear(){
                sessionStorage.clear()
                this.$router.push('/login')
            },
            findallemp(){
                //获取所有员工的信息
                this.$axios.get('http://127.0.0.1:8000/api/emp/').then(res=>{
                    console.log(res.data.results);
                    this.emp_list=res.data.results;
                }).catch(error=>{
                    this.$message('查询出错了')
                })
            },
            deleteEmp(id,index){
                //通过id删除
                //alert(id)
                this.$axios.delete('http://127.0.0.1:8000/api/emp/'+id).then(res=>{
                    this.$message('删除成功')
                    this.emp_list.splice(index,1)
                }).catch(error=>{
                    this.$message('删除失败')
                })
            },
        }
    }
</script>

<style scoped>

</style>
