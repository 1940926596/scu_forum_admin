<template>
  <div>
    <div class="pageOne">
      <div class="post">
        <div class="content1">
          <div class="content2">
            <div style="font-size: 20px;">用户名：{{ username }}</div>
            <div style="font-size: 20px;color: rgba(0,0,0,1);margin-top: 10px">邮箱账号：{{ email }}</div>
            <div style="font-size: 20px;color: rgba(0,0,0,1);margin-top: 10px">
              用户密码：{{ password }}
            </div>
          </div>
          <div>
            <el-button @click="deleteUser">删除用户</el-button>
            <el-button @click="updateUser">修改用户</el-button>
          </div>
        </div>
      </div>

    </div>
  </div>
</template>

<script>
export default {
  name: "User",
  data() {
    return {
      messages: [{
        username: '',
        email: '',
        password: new Date().toLocaleTimeString()
      }]
    }
  },
  props: {
    user_id:{
      type: Number,
      default: 0
    },
    username: {
      type: String,
      default: ""
    },
    email: {
      type: String,
      default: ""
    },
    password: {
      type: String,
      default: ""
    }
  },
  methods:{
    deleteUser:function (){
      this.$confirm('此操作将永久删除该用户, 是否继续?', '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning',
        beforeClose: (action, instance, done) => {
          if (action === 'confirm') {
            instance.confirmButtonLoading = true;
            instance.confirmButtonText = '执行中...';
            setTimeout(() => {
              done();
              setTimeout(() => {
                instance.confirmButtonLoading = false;
              }, 300);
              this.test1()
            }, 1000);
          } else {
            done();
          }
        }
      }).then(() => {
        this.$message({
          type: 'success',
          message: '删除成功!'
        });
      }).catch(() => {
        this.$message({
          type: 'info',
          message: '已取消删除'
        });
      });
    },
    updateUser:function (){
      const self=this;
      this.$alert('修改用户名', '修改用户', {
        confirmButtonText: '确定',
        showInput:true,
        inputValue:self.username,
        beforeClose: (action, instance, done) => {
          if (action === 'confirm') {
            instance.confirmButtonLoading = true;
            instance.confirmButtonText = '执行中...';
            setTimeout(() => {
              done();
              setTimeout(() => {
                instance.confirmButtonLoading = false;
              }, 300);
              this.test2(instance.inputValue)
            }, 1000);
          } else {
            done();
          }
        },
        //关闭文本框时候的回调
        callback: action => {
          this.$message({
            type: 'info',
            message: `action: ${ action }`
          });
        }
      });
    },
    test1:function (){
      console.log("hahahaha")
      const self=this;
      self.axios.get("http://43.143.211.83:8080/forum_user_delete?forum_user_name="+this.username).then(() => {
        location.reload()
      });
    },
    test2:function (changeName){
      const self=this;
      self.axios.get("http://43.143.211.83:8080/forum_user_update?forum_id="+this.user_id+"&forum_name="+changeName).then(() => {
        location.reload()
      });
    }
  },
}
</script>

<style scoped>
.pageOne {
  margin-right: 50px;
  margin-left: 50px;
  /*border: 2px solid red;*/
  display: flex;
  justify-content: center;
  flex-direction: column;
  align-items: center;

}

.content1 {
  width: 85%;
  height: 100%;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.content2 {
  padding: 20px;
  padding-top: 30px;
  height: 100%;
  width: 50%;
  position: relative;
}

.post {
  margin: 30px;
  width: 100%;
  display: flex;
  background-color: white;
  border-radius: 20px;
  box-shadow: 2px 2px 5px 3px rgba(0, 0, 0, 0.2);
}
</style>
