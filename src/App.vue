<template>
  <div id="app">
    <component
      :is="show"
      @pageChange="pageChange"
      @getUpdate="getUpdate"
      @open="open"
      @share="share"
      @loveChange="loveChange"
      @commentAdd="commentAdd"
      @setBack="setBack"
      @beBack="beBack"
      @Login="Login"
      :uid="uid"
      :mylogo="mylogo"
      :myName="myName"
      :logo="logo"
      :city="city"
      :username="user"
      :message="message"
    ></component>
  </div>
</template>

<script>
import Delivered from "./components/Delivered.vue";
import Delivering from "./components/Delivering.vue";
import Favorite from "./components/Favorite.vue";
import loading from "./components/Load.vue";
import Receive from "./components/Receive.vue";
import Login from "./components/Login.vue";
import Search from "./components/Search.vue";
import Square from "./components/Square.vue";
import Result from "./components/Result.vue";
import Throw from "./components/Throw.vue";

export default {
  data() {
    return {
      show: "Login",
      back: "Square",
      mylogo: "", //自己的头像
      logo: "", //头像
      myName: "", //自己的昵称
      user: "", //昵称
      message: "", //漂流瓶的信息打开漂流瓶是用到
      uid: "", //漂流瓶的编码
      city: "", //城市名
    };
  },
  name: "App",
  components: {
    Delivered,
    Delivering,
    Favorite,
    loading,
    Receive,
    Login,
    Search,
    Square,
    Result,
    Throw,
  },
  methods: {
    Login(logo, username) {
      this.mylogo = logo;
      this.myName = username;
    },
    pageChange(val) {
      this.show = val;
    },
    getUpdate(img, name) {
      this.logo = img;
      this.user = name;
    },
    open(uid) {
      this.uid = uid;
      this.show = "Receive";
    },
    share(img, city, message) {
      this.logo = img;
      this.city = city;
      this.message = message;
      this.show = "Result";
    },
    loveChange() {
      if (this.love == 1) {
        this.num_love--;
        this.love = 0;
      } else {
        this.num_love++;
        this.love = 1;
      }
      //及时将信息同步到后端
    },
    commentAdd() {
      this.num_comment++;
    },
    setBack(x) {
      this.back = x;
    },
    beBack() {
      this.show = this.back;
    },
  },
};
</script>

<style lang="less" scoped>
* {
  box-sizing: border-box;
}
#app {
  width: 100%;
  height: 100%;
  font-family: AlibabaPuHuiTi-3-65-Medium;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  background-color: rgba(255, 255, 255, 1);
}
</style>
