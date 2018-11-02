<template>
  <div class="user">
    <el-button size='mini' @click='toAddUser("DialogForm")'>添加</el-button>
    <!-- <el-button size='mini' @click='batchDelete'>批量删除</el-button> -->
    <el-row :gutter="20">
	  <el-col :span="6" :data="users" v-for='user in users'>
	  	<div class="user-content " >
	      <img style="width: 120px;height: 120px;border-radius:50%;margin-left:50px" :src="user.userface?user.userface:'https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1514093920321&di=913e88c23f382933ef430024afd9128a&imgtype=0&src=http%3A%2F%2Fp.3761.com%2Fpic%2F9771429316733.jpg'" alt="图片找不到了">
	    	<div class="caption">
		      	<el-row>
				  <el-col :span="6" :offset="2">用户名</el-col>
				  <el-col :span="10">{{user.username}}</el-col>
				</el-row>
		        <el-row>
				  <el-col :span="6" :offset="2">真实姓名</el-col>
				  <el-col :span="10">{{user.nickname}}</el-col>
				</el-row>
				<el-row>
				  <el-col :span="6" :offset="2">email</el-col>
				  <el-col :span="10">{{user.email}}</el-col>
				</el-row>
				<el-row>
				  <el-col :span="6" :offset="2">状态</el-col>
				  <el-col :span="10">
		        	<el-switch v-model="user.enabled" active-color="#13ce66" inactive-color="#ff4949"> </el-switch>
				  </el-col>
				</el-row> 
	  		</div>
	  	</div>
	  </el-col>
	</el-row>
	<!-- 模态框 -->
		<el-dialog width="30%"  :title="uDialog.title" :visible.sync="uDialog.visible">
		  <el-form :model="uDialog.form" status-icon ref="DialogForm" :rules="rules" size="mini" label-position="left" >
		    <el-form-item label="用户名" label-width="6em" prop="username">
		      <el-input v-model="uDialog.form.username" autocomplete="off" ></el-input>
		    </el-form-item>
		    <el-form-item label="密码" label-width="6em" prop="password">
		      <el-input type="password" v-model="uDialog.form.password" autocomplete="off"></el-input>
		    </el-form-item>
		    <el-form-item label="重复密码" label-width="6em" prop="password1">
		      <el-input type="password" v-model="uDialog.form.password1" autocomplete="off"></el-input>

		    </el-form-item>
		    <el-form-item label="真实姓名" label-width="6em" prop="nickname">
		      <el-input v-model="uDialog.form.nickname" autocomplete="off"></el-input>
		    </el-form-item>
		    <el-form-item label="email" label-width="6em" prop="email">
		      <el-input v-model="uDialog.form.email" autocomplete="off"></el-input>
		    </el-form-item>
		  </el-form>
		  <div slot="footer" class="dialog-footer">
		  	<span>{{message}}</span>
		    <el-button size='small' @click='closeuDialog'>取 消</el-button>
		    <el-button type="primary" size='small' @click='saveOrUpdateUser("DialogForm")'>确 定</el-button>
		  </div>
		</el-dialog>
		<!-- 模态框结束 -->
  </div>
</template>
<script>
	import axios from '@/http/axios';
  export default {
    methods:{
      handleSelectionChange(val) {
        this.multipleSelection = val;
      },
      
      /*提交保存或者更新栏目表单*/
		saveOrUpdateUser(formName){
			this.$refs[formName].validate((valid) => {
          		if (valid) {
					axios.post('/manager/user/saveOrUpdateUser',this.uDialog.form)
					.then(()=>{
						this.$notify.success({
				          title: '成功',
				          message: '提交成功！'
				        });
				        this.closeuDialog();
				        this.loadAllUsers();
					})
					.catch(()=>{
						this.$notify.error({
				          title: '错误',
				          message: '提交失败！'
				        });
					});
				} else {
		            return false;
		          }
		        });
		} ,
		/*关闭模态框*/ 
		closeuDialog(){
			this.uDialog.visible = false;
		},
		/* 弹出新增栏目模态框 */ 
		toAddUser(formName){
			this.uDialog.title = '添加用户';
			this.uDialog.visible = true;
			this.$refs[formName].resetFields();
		},
      loadAllUsers(){
      	axios.get('/manager/user/findAllUser')
        .then(({data:result})=>{
          this.users = result.data;
        }).catch(()=>{
			this.$notify.error({
		      title: '错误',
		      message: '网络异常！'
		    });
		})
      },
    },
    created(){
      this.loadAllUsers();
    },
    data(){
      var validatePass1 = (rule, value, callback) => {
        if (value !== this.uDialog.form.password) {
          callback(new Error('两次输入密码不一致!'));
        } else {
          callback();
        }
      };
      return {
      	uDialog:{
			title:'',
			visible:false,
			form:{}
		},
        users:[],
        multipleSelection:[],
        message:'',
        rules: {
              username: [
                { required: true, message: '请输入用户名', trigger: 'blur' },
                { min: 3, max: 12, message: '长度在 3 到 12 个字符', trigger: 'blur' }
              ],
              password: [
              	{ required: true, message: '请输入密码', trigger: 'blur' },
                { min: 6, max: 16, message: '长度在 6 到 16 个字符', trigger: 'blur' }
              ], 
              password1: [
                { required: true, message: '请再次输入密码', trigger: 'blur' },
                { validator: validatePass1, trigger: 'blur' },
                { min: 6, max: 16, message: '长度在 6 到 16 个字符', trigger: 'blur' }
              ],                           
              nickname: [
                { required: true, message: '请输入真实姓名', trigger: 'blur' },
                { min: 2, max: 10, message: '长度在 2 到 10 个字符', trigger: 'blur' }
              ],  
              email:[
            { required: true, message: '请输入邮箱地址', trigger: 'blur' },
            { type: 'email', message: '请输入正确的邮箱地址', trigger: ['blur', 'change'] }
          ]
        }
      }
    }
  }
</script>
<style>
.user{
	background-color: #ffffff;
}
	.el-button{
		margin: 1em;
	}
	.el-row {
    margin-bottom: 20px;
    &:last-child {
      margin-bottom: 0;
    }
  }
  .el-col {
    border-radius: 4px;
  }
  .user-content{
    margin:10px;
  	width: 230px;
  	height: 300px;
  	border-radius: 15px;
	border: 1px solid #ccc;
  }
  .dialog-footer>span{
  	color:red;
  	font-size: 12px;
  }
</style>