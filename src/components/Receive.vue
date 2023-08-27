<template>
  <div>
    <div class="total">
      <div class="container">
        <button class="button1" @click="beBack">&lt;</button>
      </div>
      <button class="share" @click="toShare">
        <img src="@/images/Share.png" alt="" />
      </button>
      <div class="gz">
        <h4>From {{ city }}</h4>
      </div>

      <div class="icon">
        <div class="left">
          <img :src="logo" class="logo" />
        </div>
        <div class="right">
          <p class="p1">
            <b>{{ username }}</b>
          </p>
          <p class="p2">{{ time }}</p>
        </div>
      </div>
      <div class="para">
        <p>{{ message }}</p>
      </div>
      <div class="btn">
        <button class="like" @click="loveChange">
          <img src="@/images/ht.png" class="pic1" v-if="love == 1" />
          <img src="@/images/ht_no.png" alt="" v-else class="pic1" />
          <p class="pl1">{{ num_love }}</p>
        </button>
        <button class="like" @click="Comment">
          <img src="@/images/yan.png" class="pic2" />
          <p class="pl2">{{ num_comment }}</p>
        </button>
      </div>

      <div class="comm">
        <input
          type="text"
          class="input-box"
          placeholder="写下你的评论"
          @keyup.enter="submit"
          ref="input"
        />
      </div>
      <div class="peop">
        <Comment
          v-for="(item, index) in Comments"
          :key="index"
          :img="item.img"
          :username="item.username"
          :time="item.time"
          :Comment="item.comment"
        ></Comment>
        <!-- <div class="c1">
          <div class="lay1">
            <img src="@/images/head.png" />
            <p><b>Gwen</b></p>
            <p class="t2">30分钟前</p>
          </div>
          <div class="lay2">
            <p class="pp"><b>一整个大无语</b></p>
          </div>
        </div>
        <div class="c1">
          <div class="lay1">
            <img src="@/images/head.png" />
            <p><b>Mtro</b></p>
            <p class="t2">55分钟前</p>
          </div>
          <div class="lay2">
            <p class="pp"><b>你先得这样，然后再这样，最后你再这样。。。</b></p>
          </div>
        </div> -->
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Comment from "@/components/Comment.vue";
export default {
  components: {
    Comment,
  },
  data() {
    return {
      back: "Square",
      logo: "", //写漂流瓶的用户的头像
      username: "1111", //用户名
      city: "广州",
      message:
        "1111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111", //漂流瓶的正文
      love: 0, //该用户是否已点赞
      num_love: 0, //总点赞数
      num_comment: 0, //总评论数
      time: "2023-4-5", //漂流瓶发布的时间
      Comments: [],
    };
  },
  props: {
    uid: {},
    mylogo: {},
    myName: {},
  },
  methods: {
    beBack() {
      this.$emit("beBack");
    },
    toShare() {
      this.$emit("share", this.logo, this.city, this.message);
    },
    loveChange() {
      if (this.love == 0) {
        this.love = 1;
        this.num_love++;
      } else {
        this.love = 0;
        this.num_love--;
      }
      const vue = this;

      var config = {
        method: "post",
        url: "/api/loveChange",
        headers: {
          "Content-Type": "application/json",
        },
        params: {
          uid: vue.uid,
          username: vue.myName,
          love: vue.love,
          img: vue.mylogo,
        },
      };

      axios(config).then((res) => {
        console.log(res);
      });
    },
    Comment() {
      this.$refs.input.focus(); //使得输入框获得焦点
    },
    submit() {
      if (this.$refs.input.value != "") {
        const vue = this;
        this.num_comment++;
        const x = {
          img: this.mylogo,
          username: this.myName,
          time: new Date(),
          comment: vue.$refs.input.value,
        };
        this.Comments.push(x);
        //更新了评论数，和新增了一条评论

        //将更新信息同步到后端

        var config = {
          method: "post",
          url: "/api/CommentAdd",
          headers: {
            "Content-Type": "application/json",
          },
          params: {
            uid: vue.uid,
            img: vue.mylogo,
            username: vue.myName,
            time: x.time,
            comment: x.comment,
          },
        };
        axios(config).then(() => {
          vue.$refs.input.value = "";
        });
      }
    },
  },
  mounted() {
    var vue = this;
    var uid = this.uid;

    var config = {
      method: "get",
      url: "/api/getBottleByUid",
      params: {
        uid: uid,
      },
    };

    axios(config)
      .then(function (response) {
        // console.log(JSON.stringify(response.data));
        var res = response.data;
        vue.city = res.city;
        vue.logo = res.img;
        vue.username = res.username;
        vue.time = res.time;
        vue.message = res.message;
        if (res.LoveThis.includes(vue.myName)) {
          vue.love = 1;
        } else vue.love = 0;
        vue.num_love = res.num_love;
        vue.num_comment = res.num_comment;
        vue.Comments = res.CommentThis;
      })
      .catch(function (error) {
        console.log(error);
      });
  },
};
</script>

<style lang="less" scoped>
.total {
  margin: 0 5vw;
  display: flex;
  flex-direction: column;
  height: 100%;
}

.container {
  width: 20vw;
  /* 垂直居中对齐 */
  height: 5vh;
  font-size: 2rem;
}
.share {
  position: absolute;
  top: 1vh;
  right: 2vw;
  width: 15vw;
  border: none;
  background-color: transparent;
  img {
    width: 80%;
  }
}
.button1 {
  position: absolute;
  left: 2vw;
  top: 3vw;
  border: none;
  padding: 0;
  background-color: transparent;
  color: rgb(44, 113, 134);
  font-size: 3.5rem;
}

.gz {
  display: flex;
  margin-top: 3vh;
  width: 100%;
  /* 垂直居中对齐 */
  height: 5vh;
}

.icon {
  margin-top: 7%;
  width: 100%;
  height: 5vh;
  display: flex;
  align-items: center;
}

.left {
  height: 100%;
  width: 12%;
  margin-right: 2%;
}

.left img {
  height: 100%;
  width: 100%;
  border-radius: 25%;
}

.right {
  height: 100%;
  width: 80%;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
}

.p1 {
  height: 50%;
  margin-bottom: 1%;
  margin: 0;
}

.p2 {
  height: 50%;
  margin: 0;
  color: rgb(44, 113, 134);
}

.para {
  margin-top: 1%;
  margin-bottom: 1%;
  text-align: left;
  word-wrap: break-word;
}
.btn {
  display: flex;
  flex-direction: row;
}
.like {
  width: 15%;
  height: 3vh;
  background-color: transparent;
  border: none;
  display: flex;
  font-size: 1.5rem;
}

.pic1 {
  margin: 0;
  width: 8vw;
}

.pl1 {
  margin: 0;
  margin-left: 4vw;
}

.pic2 {
  margin: 0;
  margin-left: 4vw;
  width: 8vw;
}

.pl2 {
  margin: 0;
  margin-left: 4vw;
}

.comm {
  margin-top: 2vh;
  height: 5vh;
  width: 90%;
}

.input-box {
  height: 100%;
  width: 106%;
  outline: rgb(44, 113, 134);
  border: solid rgb(44, 113, 134);
  border-radius: 1rem;
  color: gray;
  margin-top: 3vh;
  padding-left: 3vw;
}
.peop {
  margin-top: 2vh;
}
</style>