<template>
  <div class="whole" id="navi" style="width:100%;">
    <a href="/"><img src="/static/pic/logo.png" id="naviLogo" class="img-responsive logo" alt="Cinque Terre"></a>

    <nav class="navbar navbar-inverse wholeNav">
      <div class="container-fluid" style="width: 100%">
        <div class="collapse navbar-collapse" id="myNavbar" style="display: inline-block;font-size: 16px;width: 100%">
          <ul class="nav navbar-nav" id="nav" style="width: 100%">
            <li><a href="/user/restaurantList">点餐大厅</a></li>
            <li><a href="/user/orderList">我的订单</a></li>
            <li><a href="/user/personalCenter">个人中心</a></li>
            <li><a href="/guide">平台指南</a></li>
            <li id="last" style="float: right;min-width: 8%;">
              <a id="signup" href="/signup">注册</a>
              <a id="logout" v-on:click="logout" style="display: none">退出</a>
            </li>
            <li id="secondLast" style="float: right;min-width: 8%;">
              <a id="login" href="/login">登录</a>
              <a id="logoff" v-on:click="logoff" style="display: none">注销</a>
            </li>
          </ul>
        </div>
      </div>
    </nav>
  </div>
</template>

<script>
  import Avatar from "vue-avatar/src/Avatar";
  import store from '../vuex/store'
  import * as types from '../vuex/types'

  export default {
    components: {Avatar},
    inject:['reload'],
    name: 'navi',
    mounted: function () {
      let token = localStorage.getItem('Authorization');

      if (token !== 'null' || token !== ''){
        $('#login').css("display","none");
        $('#signup').css("display","none");
        $('#logout').css("display","inherit");
        $('#logoff').css("display","inherit");


      }
    },
    methods: {
      logout: function () {
        localStorage.removeItem('Authorization');
        this.$router.replace("/login");
      },
      logoff: function () {
        this.$confirm('此操作将永久注销账户, 是否继续?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          this.$message({
            type: 'success',
            message: '注销成功!'
          });
          localStorage.removeItem('Authorization');
          this.$axios.post("/user/logoff",
            {"uid":localStorage.uid}
          ).then(res => {
            let data=res.data;
            if(data){
              console.log("logout success")
            }
            this.$router.replace("/login");

          }).catch(err => {
            console.log(err)
          });
        }).catch(() => {
          this.$message({
            type: 'info',
            message: '已取消注销'
          });
        });
      }

    }
  }
</script>



<style scoped>
  .whole{
    padding: 0;
    background-color: black;
    z-index: 2;
  }
  .wholeNav{
    border-color: transparent;
    display: inline-block;
    width: 77%;
    min-width: 80%;
    min-height: 30px;
    margin-top: 5px;
    margin-bottom: 0;
    background-color: black;
  }
</style>

<style>
  #naviLogo{
    max-width: 7%;
    display: inline-block;
    margin-left: 9%;
    margin-top: -53px;
    position: relative;
  }
  #nav li{
    min-width: 10%;
    text-align: center;
    border-top:3px solid transparent;
    border-bottom:3px solid transparent;
  }
  #nav li:hover{
    border-top:3px solid dodgerblue;
    border-bottom:3px solid dodgerblue;
  }
  #nav li:hover a{
    color:dodgerblue;
  }
  #nav li a{
    color: white;
    cursor: pointer;
  }
</style>
