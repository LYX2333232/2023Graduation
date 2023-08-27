<template>
  <div class="mainDiv">
    <div class="headDiv">
      <button class="rtnBtn" @click="beBack">&lt;</button>
      <div class="headDivHor">
        <h1 class="headTXT">我的漂流瓶</h1>
        <img class="btImg" src="@/images/my.png" alt="漂流瓶" />
      </div>
    </div>
    <div class="tableDiv">
      <div class="tableEDiv" refs="tableEDiv">
        <!-- <p v-html="add"></p> -->
        <OrderByCity
          v-for="(item, index) in items"
          :key="index"
          :city="item.city"
          :items="item.items"
          @open="open"
        ></OrderByCity>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import OrderByCity from "@/components/OrderByCity.vue";
export default {
  data() {
    return {
      back: "Square",
      citys: [],
      items: [], //记录，里面用img记录头像图片，用city记录城市名，用message记录漂流瓶内容，username为用户名
    };
  },
  props: {
    uid: {},
    myName: {},
  },
  methods: {
    beBack() {
      this.$emit("pageChange", this.back);
    },
    open(uid) {
      this.$emit("setBack", "Delivered");
      this.$emit("open", uid);
    },
  },
  components: {
    OrderByCity,
  },
  mounted() {
    //请求数据
    //先按请求城市
    var username = this.myName;
    var vue = this;

    var config = {
      method: "get",
      url: "/api/getMyCity",
      params: {
        username: username,
      },
    };

    axios(config)
      .then(function (response) {
        console.log(JSON.stringify(response.data));
        vue.citys = response.data.citys;

        //根据城市请求具体信息

        vue.citys.forEach((city) => {
          var config = {
            method: "get",
            url: "/api/getMyBottlesByCity",
            params: {
              username: vue.myName,
              city: city,
            },
          };

          axios(config)
            .then(function (response) {
              console.log(JSON.stringify(response.data));
              vue.items.push(response.data);
            })
            .catch(function (error) {
              console.log(error);
            });
        });
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
div.headDivHor {
  display: flex;
  justify-content: flex-start;
  width: 100%;
}
h1.headTXT {
  color: rgb(44, 113, 134);
  width: 80%;
  margin: 0px;
  margin-top: 10vh;
  display: inline-block;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}
img.btImg {
  width: 20%;
  height: 20%;
  margin-top: 5vh;
}
div.tableDiv {
  width: 100%;
}
div.tableEDiv {
  width: 100%;
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
.tblImg {
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
</style>
