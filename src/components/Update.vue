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
                        <a href="#">Main</a>
                    </h1>
                </div>
                <div id="navigation">
                </div>
            </div>
            <div id="content">
                <p id="whereami">
                </p>
                <h1>
                    update Emp info:
                </h1>
                <form action="emplist.html" method="post">
                    <table cellpadding="0" cellspacing="0" border="0"
                           class="form_table">
                        <tr>
                            <td valign="middle" align="right">
                                id:
                            </td>
                            <td valign="middle" align="left">
                                {{$route.query.id}}
                            </td>
                        </tr>
                        <tr>
                            <td valign="middle" align="right">
                                name:
                            </td>
                            <td valign="middle" align="left">
                                <input type="text" class="inputgri" name="name" v-model="emp_name"/>
                            </td>
                        </tr>
                        <tr>
                            <td valign="middle" align="right">
                                photo:
                            </td>
                            <td valign="middle" align="left">
                                <input type="file" name="photo" ref="image"/>
                            </td>
                        </tr>
                        <tr>
                            <td valign="middle" align="right">
                                salary:
                            </td>
                            <td valign="middle" align="left">
                                <input type="text" class="inputgri" name="salary" v-model="salary"/>
                            </td>
                        </tr>
                        <tr>
                            <td valign="middle" align="right">
                                age:
                            </td>
                            <td valign="middle" align="left">
                                <input type="text" class="inputgri" name="age" v-model="age"/>
                            </td>
                        </tr>
                    </table>
                    <p>
                        <el-button type="success" @click="updateEmp">提交修改</el-button>
                    </p>
                </form>
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
        name: "Update",
        data(){
            return {
                emp_name: '',
                img: '',
                salary: '',
                age: '',
            }
        },
        methods:{
            updateEmp(){
                let formData=new FormData();
                console.log(this.emp_name)
                formData.append('emp_name',this.emp_name)
                formData.append('img',this.$refs.image.files[0])
                formData.append('salary',this.salary)
                formData.append('age',this.age)
                let id=this.$route.query.id
                this.$axios({
                    url: 'http://127.0.0.1:8000/api/emp/'+id,
                    method: 'put',
                    data: formData,
                    headers:{
                        'content-type':'multipart/form-data'
                    }
                }).then(res => {
                    this.$message('修改成功')
                    //添加成功返回员工列表
                    this.$router.push('/index')
                }).catch(error => {
                    this.$message.error('修改失败')
                })
            }
        },
        created(){
            this.$axios.get('http://127.0.0.1:8000/api/emp/'+this.$route.query.id).then(res=>{
                    this.emp_name=res.data.results.emp_name
                    this.salary=res.data.results.salary
                    this.age=res.data.results.age
                }).catch(error=>{
                    this.$message('查询出错了')
                })
        }
    }
</script>

<style scoped>

</style>
