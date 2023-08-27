<template>
  <div class="container">
    <div class="background"></div>
    <div class="content">
      <div class="word">毕业<br />都去哪儿了</div>
      <div class="search-box">
        <img src="@/images/search.png" @click="toSearch" />
        <!-- <span class="search-icon" @click="toSearch()"> </span> -->
        <!-- <input type="text" placeholder="搜索..."> -->
        <!-- <button type="submit">搜索</button> -->
      </div>
      <h3 class="title">漂流广场</h3>
      <h6 class="hi">这里有6个随机的漂流瓶，快点开来看看是什么吧</h6>
      <div class="image-row">
        <button class="look" @click="toReceive0">
          <img src="@/images/bottle.png" />
          <h4 class="city">{{ city0 }}</h4>
          <div class="user">{{ user0 }}</div>
        </button>
        <button class="look" @click="toReceive1">
          <img src="@/images/bottle.png" />
          <h4 class="city">{{ city1 }}</h4>
          <div class="user">{{ user1 }}</div>
        </button>
        <button class="look" @click="toReceive2">
          <img src="@/images/bottle.png" />
          <h4 class="city">{{ city2 }}</h4>
          <div class="user">{{ user2 }}</div>
        </button>
      </div>
      <div class="image-row">
        <button class="look" @click="toReceive3">
          <img src="@/images/bottle.png" />
          <h4 class="city">{{ city3 }}</h4>
          <div class="user">{{ user3 }}</div>
        </button>
        <button class="look" @click="toReceive4">
          <img src="@/images/bottle.png" />
          <h4 class="city">{{ city4 }}</h4>
          <div class="user">{{ user4 }}</div>
        </button>
        <button class="look" @click="toReceive5">
          <img src="@/images/bottle.png" />
          <h4 class="city">{{ city5 }}</h4>
          <div class="user">{{ user5 }}</div>
        </button>
      </div>
      <button class="change" @click="getSome">
        <img src="@/images/change.png" alt="" />
        <div class="words">换一批</div>
      </button>
      <div class="column-row">
        <div class="column">
          <img src="@/images/send.png" @click="toDelivering" />
          <p>投送漂流瓶</p>
        </div>
        <div class="spare"></div>
        <div class="column">
          <img src="@/images/like.png" @click="toFavorite" />
          <p>我喜欢的和评论</p>
        </div>
        <div class="spare"></div>
        <div class="column">
          <img src="@/images/mine.png" @click="toDelivered" />
          <p>我的漂流瓶</p>
        </div>
      </div>
      <!-- <button @click="toSearch">搜索</button> -->
      <!-- <button @click="toDelivering">投稿</button>
        <button @click="toReceive">查看漂流瓶</button>
        <button @click="toDelivered">我投递过的</button>
        <button @click="toFavorite">我收藏的</button> -->
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      search: "Search",
      delivering: "Delivering",
      delivered: "Delivered",
      favorite: "Favorite",
      receive: "Receive",
      city0: "11",
      city1: "",
      city2: "",
      city3: "",
      city4: "",
      city5: "",
      user0: "11",
      user1: "",
      user2: "",
      user3: "",
      user4: "",
      user5: "",
      uid0: 1,
      uid1: 2,
      uid2: 3,
      uid3: 4,
      uid4: 5,
      uid5: 6,
      logo0: "",
      logo1: "",
      logo2: "",
      logo3: "",
      logo4: "",
      logo5: "",
      //城市、名称和记录的编号都从数据库随机获取
    };
  },
  methods: {
    toSearch() {
      this.$emit("pageChange", this.search);
    },
    toDelivering() {
      this.$emit("pageChange", this.delivering);
    },
    toReceive0() {
      this.$emit("setBack", "Square");
      this.$emit("open", this.uid0);
    },
    toReceive1() {
      this.$emit("setBack", "Square");
      this.$emit("open", this.uid1);
    },
    toReceive2() {
      this.$emit("setBack", "Square");
      this.$emit("open", this.uid2);
    },
    toReceive3() {
      this.$emit("setBack", "Square");
      this.$emit("open", this.uid3);
    },
    toReceive4() {
      this.$emit("setBack", "Square");
      this.$emit("open", this.uid4);
    },
    toReceive5() {
      this.$emit("setBack", "Square");
      this.$emit("open", this.uid5);
    },
    toDelivered() {
      this.$emit("setBack", "Square");
      this.$emit("pageChange", this.delivered);
    },
    toFavorite() {
      this.$emit("setBack", "Square");
      this.$emit("pageChange", this.favorite);
    },
    getSome() {
      var this_vue = this;
      var sum;
      //获取6个在sum范围内的漂流瓶
      var config1 = {
        method: "get",
        url: "/api/sum",
        // headers: {
        //   "User-Agent": "Apifox/1.0.0 (https://apifox.com)",
        // },
      };

      axios(config1)
        .then(function (response) {
          // console.log(response);
          sum = response.data.sum;
          // console.log(sum);

          var config = {
            method: "get",
            url: "/api/getid",
            params: {
              max: sum,
            },
            // headers: {
            //   "User-Agent": "Apifox/1.0.0 (https://apifox.com)",
            // },
          };

          axios(config)
            .then(function (response) {
              // console.log(JSON.stringify(response.data));
              this_vue.uid0 = response.data.uid0;
              this_vue.uid1 = response.data.uid1;
              this_vue.uid2 = response.data.uid2;
              this_vue.uid3 = response.data.uid3;
              this_vue.uid4 = response.data.uid4;
              this_vue.uid5 = response.data.uid5;

              var config0 = {
                method: "get",
                url: "/api/getBottleByUid",
                params: {
                  uid: this_vue.uid0,
                },
              };
              var config1 = {
                method: "get",
                url: "/api/getBottleByUid",
                params: {
                  uid: this_vue.uid1,
                },
              };
              var config2 = {
                method: "get",
                url: "/api/getBottleByUid",
                params: {
                  uid: this_vue.uid2,
                },
              };
              var config3 = {
                method: "get",
                url: "/api/getBottleByUid",
                params: {
                  uid: this_vue.uid3,
                },
              };
              var config4 = {
                method: "get",
                url: "/api/getBottleByUid",
                params: {
                  uid: this_vue.uid4,
                },
              };
              var config5 = {
                method: "get",
                url: "/api/getBottleByUid",
                params: {
                  uid: this_vue.uid5,
                },
              };

              axios(config0)
                .then(function (response) {
                  // console.log(JSON.stringify(response.data));
                  this_vue.city0 = response.data.city;
                  this_vue.user0 = response.data.username;
                })
                .catch(function (error) {
                  console.log(error);
                });
              axios(config1)
                .then(function (response) {
                  // console.log(JSON.stringify(response.data));
                  this_vue.city1 = response.data.city;
                  this_vue.user1 = response.data.username;
                })
                .catch(function (error) {
                  console.log(error);
                });
              axios(config2)
                .then(function (response) {
                  // console.log(JSON.stringify(response.data));
                  this_vue.city2 = response.data.city;
                  this_vue.user2 = response.data.username;
                })
                .catch(function (error) {
                  console.log(error);
                });
              axios(config3)
                .then(function (response) {
                  // console.log(JSON.stringify(response.data));
                  this_vue.city3 = response.data.city;
                  this_vue.user3 = response.data.username;
                })
                .catch(function (error) {
                  console.log(error);
                });
              axios(config4)
                .then(function (response) {
                  // console.log(JSON.stringify(response.data));
                  this_vue.city4 = response.data.city;
                  this_vue.user4 = response.data.username;
                })
                .catch(function (error) {
                  console.log(error);
                });
              axios(config5)
                .then(function (response) {
                  // console.log(JSON.stringify(response.data));
                  this_vue.city5 = response.data.city;
                  this_vue.user5 = response.data.username;
                })
                .catch(function (error) {
                  console.log(error);
                });
            })
            .catch(function (error) {
              console.log(error);
            });
        })
        .catch(function (error) {
          console.log(error);
        });
      //取得目前已有的漂流瓶总数
    },
  },
  created() {
    this.getSome();
  },
};
</script>

<style lang="less" scoped>
.container {
  display: flex;
  background-image: url("@/images/up.png");
  background-repeat: no-repeat;
  background-size: 100%;
  height: 100vh;
  flex-direction: column;
  position: relative;
}

.background {
  height: 30%;
  background-size: cover;
  background-repeat: no-repeat;
  background-position: center top;
}

.content {
  height: 70%;
  background-color: white;
  position: relative;
  /* height: 55%; */
  margin-top: 1%;
  border-radius: 5% 5% 0 0;
  .word {
    margin-top: -40%;
    margin-left: 5%;
    font-size: 3.5rem;
    text-align: left;
    color: white;
  }
}

.search-box {
  background-size: cover;
  background-repeat: no-repeat;
  background-position: center;
  padding: 10px;
  margin-top: 3vh;
}

.search-box img {
  width: 94%;
  /* Adjust the width as needed */
  object-fit: cover;
  border-radius: 1.2rem;
}

.search-box .search-icon {
  width: 40px;
  height: 40px;
  /* background-color: red; */
  background-image: url("@/images/icon.png");
  /* 替换为你的图片路径 */
  background-size: cover;
  background-repeat: no-repeat;
  background-position: center;
  margin-right: 10px;
  cursor: pointer;
}

.search-box input[type="text"] {
  width: 100%;
  /* padding: 8px; */
  border: none;
  border-radius: 5px;
  font-size: 16px;
}

.title {
  color: rgb(44, 113, 134);
  text-align: left;
  margin: 0;
  margin-left: 7vw;
  font-size: 1.8rem;
}
.hi {
  margin: 0;
  margin-left: 7vw;
  text-align: left;
  font-size: 1rem;
  font-weight: 100;
}
.image-row {
  display: flex;
  justify-content: space-evenly;
  margin-top: 20px;
}

.look {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 25vw;
  height: 35vw;
  /* Adjust the width as needed */
  object-fit: cover;
  border: none;
  border-radius: 10%;
  box-shadow: 0 0 2.3vw rgba(0, 0, 0, 0.25);
  background-color: rgba(243, 238, 205, 0.55);
  img {
    margin-top: 5vw;
    width: 50%;
  }
  .city {
    margin: 1vw;
  }
  .user {
    width: 100%;
    display: inline-block;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
  }
}
.change {
  display: flex;
  border: none;
  text-align: left;
  height: 3vh;
  border-radius: 0.7rem;
  margin: 3vh auto 0;
  margin-top: 3vh;
  font-size: 1rem;
  flex-direction: row;
  img {
    width: 5vw;
  }
}
.column-row {
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 100%;
  height: 12%;
  margin-top: 2vh;
  border-top-left-radius: 8vw;
  border-top-right-radius: 8vw;
  box-shadow: 0 0 5vw rgba(0, 0, 0, 0.25);
}

.column {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: calc(33.33% - 10px);
  /* Adjust the width as needed */
  height: 100%;
  padding: 10px;
}
.spare {
  background-color: rgba(166, 166, 166, 1);
  width: 0.1%;
  height: 55%;
}
.column img {
  margin-top: 1vh;
  width: 60%;
  height: 60%;
  object-fit: cover;
  /* border-radius: 5px; */
  /* margin-bottom: 10px; */
}

.column p {
  height: 10%;
  text-align: center;
  margin-top: auto;
  font-size: 0.8em;
}
</style>