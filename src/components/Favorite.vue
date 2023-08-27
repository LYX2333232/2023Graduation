<template>
  <div class="mainDiv">
    <div class="headDiv">
      <button class="rtnBtn" @click="beBack">&lt;</button>
      <h1 class="headTXT">我的评论&amp;喜欢</h1>
    </div>
    <div class="tableDiv">
      <div class="tableEDiv">
        <div class="tableHDiv">
          <h2 class="tblHTXT">喜欢</h2>
          <img class="tblHImg" src="@/images/love.png" alt="喜欢" />
        </div>
        <Bottle
          v-for="(item, index) in items_love"
          :key="index"
          :uid="item.uid"
          :img="item.img"
          :city="item.city"
          :time="item.time"
          :userName="item.username"
          :message="item.message"
          @open="open"
        ></Bottle>
        <!-- <button class="botBtn">
          <div class="bottDiv">
            <img
              class="tblImg"
              src="https://cannotloadname.github.io/chinaz_icons/5426/36.png"
              alt="漂流瓶"
            />
            <div class="tblFrm" ref="goto" @click="open">
              <div class="tblFrmIn">
                <h3 class="tblName">泰酷辣</h3>
                <h4 class="tblTime">2023年6月10日</h4>
              </div>
              <h4 class="tblInc">如果你也和我一样，那我只能…</h4>
            </div>
          </div>
        </button>
        <button class="botBtn">
          <div class="bottDiv">
            <img
              class="tblImg"
              src="https://cannotloadname.github.io/chinaz_icons/5426/40.png"
              alt="漂流瓶"
            />
            <div class="tblFrm">
              <div class="tblFrmIn">
                <h3 class="tblName">泰酷辣</h3>
                <h4 class="tblTime">2023年6月10日</h4>
              </div>
              <h4 class="tblInc">如果你也和我一样，那我只能…</h4>
            </div>
          </div>
        </button> -->
        <div class="tableHDiv">
          <h2 class="tblHTXT">评论</h2>
          <img class="tblHImg" src="@/images/comment.png" alt="评论" />
        </div>
        <BottleWithCom
          v-for="(item, index) in items_comments"
          :key="index"
          :uid="item.uid"
          :img="item.img"
          :city="item.city"
          :time="item.time"
          :userName="item.username"
          :message="item.message"
          :myComments="item.myComments"
          @open="open"
        ></BottleWithCom>
        <!-- <button class="botBtn">
          <div class="bottDiv">
            <img
              class="tblImg"
              src="https://cannotloadname.github.io/chinaz_icons/5426/51.png"
              alt="漂流瓶"
            />
            <div class="tblFrm">
              <div class="tblFrmIn">
                <h3 class="tblName">泰酷辣</h3>
                <h4 class="tblTime">2023年6月10日</h4>
              </div>
              <h4 class="tblInc">如果你也和我一样，那我只能…</h4>
            </div>
          </div>
        </button>
        <button class="comBtn">
          <div class="comDiv">
            <h4 class="comTXT">你得先这样，然后再这样，最后你再这样。。。</h4>
          </div>
        </button>
        <button class="botBtn">
          <div class="bottDiv">
            <img
              class="tblImg"
              src="https://cannotloadname.github.io/chinaz_icons/5426/52.png"
              alt="漂流瓶"
            />
            <div class="tblFrm">
              <div class="tblFrmIn">
                <h3 class="tblName">泰酷辣</h3>
                <h4 class="tblTime">2023年6月10日</h4>
              </div>
              <h4 class="tblInc">如果你也和我一样，那我只能…</h4>
            </div>
          </div>
        </button>
        <button class="comBtn">
          <div class="comDiv">
            <h4 class="comTXT">你得先这样，然后再这样，最后你再这样。。。</h4>
          </div>
        </button> -->
      </div>
    </div>
  </div>
</template>

<script>
import Bottle from "@/components/Bottle.vue";
import BottleWithCom from "@/components/BottleWithCom.vue";
export default {
  data() {
    return {
      back: "Square",
      look: "Receive",
      items_love: [], //记录，里面用img记录头像图片，用city记录城市名，用message记录漂流瓶内容，username为用户名
      items_comments: [],
    };
  },
  props: {
    mylogo: {},
    myName: {},
  },
  components: {
    Bottle,
    BottleWithCom,
  },
  methods: {
    beBack() {
      this.$emit("pageChange", this.back);
    },
    open(img, city, username, message) {
      this.$emit("setBack", "Favorite");
      this.$emit("open", img, city, username, message);
    },
  },
  mounted() {
    //请求数据，以myName进行检索
    //我喜欢的
    var axios = require("axios");

    var vue = this;
    var config0 = {
      method: "get",
      url: "/api/getBottleByLove",
      params: {
        username: vue.myName,
      },
    };

    axios(config0)
      .then(function (response) {
        console.log(JSON.stringify(response.data));
        vue.items_love = response.data.items;
      })
      .catch(function (error) {
        console.log(error);
      });

    //我评论过的

    var config1 = {
      method: "get",
      url: "/api/getBottleWithComment",
      params: {
        username: vue.myName,
      },
    };

    axios(config1)
      .then(function (response) {
        console.log(JSON.stringify(response.data));
        vue.items_comments = response.data.items;
      })
      .catch(function (error) {
        console.log(error);
      });
  },
};
</script>

<style lang="less" scoped>
div.mainDiv {
  font-size: 100%;
  text-align: left;
  padding-left: 6%;
  padding-right: 6%;
  padding-top: 2%;
  padding-bottom: 10%;
}
div.headDiv {
  width: 100%;
}
button.rtnBtn {
  position: absolute;
  left: 2vw;
  top: 3vw;
  border: none;
  padding: 0;
  background-color: transparent;
  color: rgb(44, 113, 134);
  font-size: 3.5rem;
}

h1.rtnTXT {
  font-size: 3.5rem;
  color: rgb(44, 113, 134);
  margin: 0px;
}

h1.headTXT {
  color: rgb(44, 113, 134);
  margin: 0;
  margin-top: 10vh;
  display: inline-block;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  width: 100%;
}

div.tableDiv {
  width: 100%;
}
div.tableEDiv {
  width: 100%;
}
div.tableHDiv {
  display: flex;
  justify-content: flex-start;
  width: 100%;
  margin-top: 6%;
  margin-bottom: 6%;
}
h2.tblHTXT {
  margin: 0px;
  display: inline-block;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  max-width: 90%;
}
img.tblHImg {
  width: 10%;
  height: 10%;
  margin-left: 5vw;
}
button.botBtn {
  background-color: transparent;
  text-align: left;
  width: 100%;
  height: auto;
  border-width: 0px;
  margin-left: 0px;
  margin-right: 0px;
  margin-top: 2%;
  margin-bottom: 2%;
  padding: 0px;
}
div.bottDiv {
  font-size: 110%;
  display: flex;
  justify-content: flex-start;
  width: 100%;
}
img.tblImg {
  width: 12%;
  height: 12%;
  border-radius: 20%;
}
div.tblFrm {
  width: 88%;
  padding-left: 2%;
}
div.tblFrmIn {
  display: flex;
  justify-content: flex-start;
  width: 100%;
}
h3.tblName {
  margin: 0px;
  width: 55%;
  display: inline-block;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}
h4.tblTime {
  text-align: right;
  color: DarkCyan;
  margin: 0px;
  width: 45%;
  display: inline-block;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}
h4.tblInc {
  margin: 0px;
  color: DarkGray;
  display: inline-block;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  width: 100%;
}
button.comBtn {
  background-color: transparent;
  text-align: left;
  width: 100%;
  height: auto;
  border-width: 0px;
  margin-left: 0px;
  margin-right: 0px;
  margin-top: 0px;
  margin-bottom: 4%;
  padding: 0px;
}
</style>
