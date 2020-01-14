<template>
  <div class="userhome">
    <div>
      <img class="userHead" :src="$store.state.user.userHead" alt />
    </div>
    <div class="username">
      {{ $store.state.user.name }}
      <a href="javascript:;" @touchstart="handleToLogout">退出</a>
    </div>

    <div class="usercut" v-if="$store.state.user.isAdmin">
      用户身份:管理员
      <a href="/admin" target="_blank">进入管理后台</a>
    </div>
    <div v-else>用户身份:普通会员</div>
    <input type="file" name="file" value="上传头像" @change="headleToUpload" />
  </div>
</template>
<script>
import axios from "axios";
import { messageBox } from "@/components/JS";
export default {
  name: "center",
  methods: {
    handleToLogout() {
      this.axios.get("/api2/users/logout").then(res => {
        // console.log(res)
        var status = res.data.status;
        if (status === 0) {
          localStorage.removeItem("name");
          localStorage.removeItem("isAdmin");
          this.$router.push("/mine/login");
          this.$store.commit("user/USER_NAME", {
            name: "",
            isAdmin: false,
            userHead: ""
          });
        }
      });
    },
    headleToUpload(ev) {
      var file = ev.target.files[0];
      var parm = new FormData();
      var config = {
        headers: {
          "Content-Type": "multipart/form-data"
        }
      };
      parm.append("file", file, file.name);
      this.axios
        .post("/api2/users/uploadUserHead", parm, config)
        .then(res => {
          var status = res.data.status;
          var This = this;
          if (status === 0) {
            messageBox({
              title: "信息",
              content: "上传头像成功",
              ok: "确定",
              handleOk() {
                This.$store.commit("user/USER_NAME", {
                  name: This.$store.state.user.name,
                  isAdmin: This.$store.state.user.isAdmin,
                  userHead: res.data.data.userHead + "?" + Math.random()
                });
              }
            });
          } else {
            messageBox({
              title: "信息",
              content: "上传头像失败",
              ok: "确定"
            });
          }
        })
        .catch(err => {
          console.log(err);
        });
    }
  },
  beforeRouteEnter(to, from, next) {
    axios.get("/api2/users/getUser").then(res => {
      var status = res.data.status;
      if (status === 0) {
        next(vm => {
          localStorage.setItem("name", res.data.data.username);
          localStorage.setItem("isAdmin", res.data.data.isAdmin);
          vm.$store.commit("user/USER_NAME", {
            name: res.data.data.username,
            isAdmin: res.data.data.isAdmin,
            userHead: res.data.data.userHead
          });
        });
      } else {
        next("/mine/login");
      }
    });
  }
};
</script>
<style scoped>
.userhome {
  height: 100%;
  display: flex;
  flex-direction: column;
  text-align: center;
  position: relative;
  padding: 20px 0;
  background-color: aliceblue;
}
.userHead {
  width: 100px;
  height: 100px;
  border-radius: 50%;
  overflow: hidden;
  margin: 0 auto;
}
.username {
  margin: 20px;
  color: #e54847;
  font-size: 26px;
  font-weight: 600;
}
.username a {
  border: 2px solid #a00;
  border-radius: 6px;
  padding: 4px 10px;
  margin-left: 25px;
  font-size: 16px;
  font-weight: 400;
  text-decoration: none;
  color: rgb(199, 0, 0);
}
.usercut {
  border-top: 1px solid #000;
  padding: 20px 0;
  color: #62ade0;
  font-size: 22px;
  font-weight: 600;
}
.usercut a {
  border: 2px solid #62ade0;
  border-radius: 6px;
  padding: 4px 10px;
  margin-left: 25px;
  font-size: 16px;
  font-weight: 400;
  text-decoration: none;
  color: #62ade0;
}
.userhome input {
  height: 60px;
}
</style>
