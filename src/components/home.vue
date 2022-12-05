<template>
  <div class="main">
    <div class="head" style="height:10%;display: flex;justify-content: center;align-items: center;">
      <img height="100%" src="../assets/header(1).jpg" width="auto"/>
      <div class="colortxt">校园论坛后台管理系统</div>
    </div>
    <div class="home" style=" height:80%; border:2px solid rgba(192,192,192,0.2)">
      <div class="sidebar">
        <div class="content1">管理选项</div>
        <!--        <div class="label1" id="creator">管理员信息</div>-->
        <div id="user" class="label1" @click="ToUser">用户信息</div>
        <div id="page" class="label1" @click="ToPage">发帖信息</div>
        <div id="message" class="label1" @click="ToMessage">回复信息</div>
      </div>
      <div class="pages">
        <user v-if="active===1" v-for="(user1,index) in User" :key='index' :user_id="user1.forum_id" :username="user1.forum_name" :email="user1.forum_email" :password="user1.forum_pwd"></user>
        <page v-if="active===2" v-for="(page1,index) in Page" :page_id="page1.page_id" :key='index' :username="page1.page_sender_name" :content="page1.page_send_content" :time="page1.page_send_time"></page>
        <message v-if="active===3" v-for="(message1,index) in Message" :message_id="message1.message_id" :key='index' :username="message1.message_sender_name" :content="message1.message_content" :time="message1.message_send_time"></message>
      </div>
    </div>
    <margin-footer></margin-footer>
  </div>
</template>

<script>
import MarginFooter from "./Individual/marginFooter";
import User from "./Individual/User";
import Message from "./Individual/Message";
import Page from "./Individual/Page";

export default {
  name: "home",
  components: {Page, Message, User, MarginFooter},
  data() {
    return {
      active:1,
      User:{
        forum_id:'',
        forum_name:'',
        forum_email:'',
        forum_pwd:''
      },
      Page:{
        page_id:'',
        page_sender_id:'',
        page_sender_name:'',
        page_send_content:'',
        page_send_time:''
      },
      Message:{
        message_id:'',
        message_content_id:'',
        message_sender_name:'',
        message_content:'',
        message_send_time:''
      }
    }
  },
  methods: {
    ToUser: function () {
      this.active=1
      const self =this
      self.axios.get("http://43.143.211.83:8080/forum_user_list").then((response) => {
        self.User = response.data
      })
    },
    ToPage: function () {
      this.active=2
      const self =this
      self.axios.get("http://43.143.211.83:8080/forum_page_list").then((response) => {
        self.Page = response.data
      })
    },
    ToMessage: function () {
      this.active=3
      const self =this
      self.axios.get("http://43.143.211.83:8080/forum_message_list").then((response) => {
        self.Message = response.data
      })
    }
  },
  mounted() {
    const self =this
    self.axios.get("http://43.143.211.83:8080/forum_user_list").then((response) => {
      self.User = response.data
    })
  },
  beforeDestroy() {
    console.log("destroy")
  }
}
</script>

<style scoped>
.main {
  width: 100%;
  height: 100%;
  background-color: rgba(211, 161, 117, 0.1);
  overflow: hidden;
}

.home {
  display: flex;
  align-items: center;
  /*background-color: rgb(255, 255, 255);*/
}

.sidebar {
  background-color: rgba(149, 62, 16, 1);
  align-items: center;
  background-color: white;
  display: flex;
  flex-direction: column;
  border: 2px solid silver;
  flex-shrink: 10;
  height: 50%;
  width: 10%;
  border-radius: 2%;
  margin-top: 10px;
  margin-left: 100px;
  /*justify-content: space-around;*/
}

.pages {
  overflow: scroll;
  overflow-x: hidden;
  border: 5px solid silver;
  margin-top: 10px;
  width: 65%;
  flex-shrink: 1;
  height: 95%;
  border-radius: 2%;
  margin-left: 100px;
  background-color: white;
}

.colortxt {
  font-size: 30px;
  background: linear-gradient(to right, red, blue);
  -webkit-background-clip: text;
  color: transparent;
}

#user {

}

#message {

}

#page {

}

.label1 {

  display: flex;
  justify-content: center;
  align-items: center;
  border: 1px solid silver;
  color: black;
  font-size: 20px;
  padding: 10px;
  height: 40px;
  margin: 5px;
  border-radius: 5px;
  margin-top: 30px;
  cursor: pointer;
}

.label1:hover {
  color: white;
  background-color: silver;
}

.content1 {
  /*border: 2px solid purple;*/
  margin-top: 10px;
  font-size: 20px;
  width: 95%;
  height: auto;
  display: flex;
  justify-content: center;
  align-items: center;
  border-bottom: 2px solid rgba(0, 0, 0, 0.2);
}

</style>
