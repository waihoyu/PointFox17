<template>
  <div id="registerWrapper">
    <div id="registerContent">
      <el-card class="box-card">
        <div slot="header" class="clearfix">
          <span>用户注册</span>
          <span
            id="checkTip"
            v-text="checktip"
            :class="flag ? tipRed : tipGreen"
          ></span>
        </div>
        <div id="registerContentBody">
          <el-input
            id="newusername"
            v-model="newUser.newusername"
            clearable
            @blur="checkUsername"
          ></el-input>
          <el-input
            id="newpassword"
            v-model="newUser.newpassword"
            show-password
            clearable
          ></el-input>
        </div>
        <!--  v-loading.fullscreen.lock="fullscreenLoading" -->
        <div id="registerSubmit">
          <el-button
            id="systemSubmit"
            @click="registerSystem"
            :disabled="this.disabled"
            >注册</el-button
          >
        </div>
      </el-card>
    </div>
  </div>
</template>

<script>
import Axios from 'axios'
const url = 'http://localhost:3000'
export default {
  name:"register",
    data(){
        return {
            disabled:true,
            newUser:{
                newusername:"",
                newpassword:""
            },
            fullscreenLoading: false,
            checktip:"",
            flag:false,
            tipRed:"tipRed",
            tipGreen:"tipGreen",
        }
    },
    methods:{
      checkUsername(){
        Axios.post(url + '/register/check',{
          params:this.newUser
        }).then((response)=>{
          console.log(response)
          if(response.data.state =="success"){            
            this.flag = false
            this.disabled = false          
          }else{
            this.flag = true
            this.disabled =true
          }
          this.checktip = response.data.message
            // if(response.data.state =="success"){
            //   this.$message('注册用户成功');
            // }
            // else{
            //    this.$message('注册用户失败');
            // }         
        })
      },
      // CORS 
      registerSystem(){  
        Axios.post(url +'/register',{
          params: this.newUser
        }).then((response)=>{
        if(response.data.state == "success"){
          console.log(response.data.state)
          this.$message("注册成功")  
          this.newUser.newusername=""
          this.newUser.newpassword=""
          this.checktip="" 
        }})
        // this.fullscreenLoading = true;
        // that = this
      }
    }
};
</script>

<style >
#registerWrapper {
  /* background-color: purple; */
  height: 100%;
}
#registerContent {
  width: 400px;
  margin: 0 auto;
  padding-top: 100px;
}
#systemSubmit {
  display: block;
  margin: 0 auto;
  background-color: #0a9588;
  color: white;
  border-color: #0a9588;
}
#newpassword {
  margin-top: 5px;
  margin-bottom: 8px;
}
#checkTip {
  /* background-color: red; */
  margin-left: 50px;
}
.tipRed {
  color: red;
}
.tipGreen {
  color: green;
}
</style>
