<template>
  <div>
    <div class="pageOne">
      <div class="post">
        <div class="content1">
          <div class="content2">
            <div style="font-size: 20px;">发帖人用户名：{{ username }}</div>
            <div style="font-size: 20px;color: rgba(0,0,0,1);margin-top: 10px">发帖内容：{{ content }}</div>
            <div style="font-size: 20px;color: rgba(0,0,0,1);margin-top: 10px">
              发帖时间：{{ time }}
            </div>
          </div>
          <div>
            <el-button @click="deletePage">删除帖子</el-button>
            <el-button @click="updatePage">修改帖子</el-button>
          </div>
        </div>
      </div>

    </div>
  </div>
</template>

<script>
export default {
  name: "Page",
  data() {
    return {
      messages: [{
        username: '',
        email: '',
        password: ''
      }]
    }
  },
  props: {
    page_id: {
      type: Number,
      default: 0
    },
    username: {
      type: String,
      default: ""
    },
    content: {
      type: String,
      default: ""
    },
    time: {
      type: String,
      default: ""
    }
  },
  methods: {
    test1: function () {
      const self = this;
      self.axios.get("http://43.143.211.83:8080/forum_page_delete?page_id=" + this.page_id).then((response) => {
        location.reload()
      })
    },
    deletePage: function () {
      this.$confirm('此操作将永久删除该帖子, 是否继续?', '提示', {
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
    updatePage:function (){
      const self=this;
      this.$alert('修改帖子内容', '修改帖子', {
        confirmButtonText: '确定',
        showInput:true,
        inputValue:self.content,
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
    test2:function (changeContent){
      const self=this;
      self.axios.get("http://43.143.211.83:8080/forum_page_update?page_id="+this.page_id+"&page_send_content="+changeContent).then(() => {
        location.reload()
      });
    }
  }
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
