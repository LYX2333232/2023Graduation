<template>
  <div class="mainDiv">
    <div class="headDiv">
      <button class="rtnBtn" @click="beBack">&lt;</button>
      <div class="headDivHor">
        <button class="searchBtn">
          <img src="@/images/searching.png" alt="搜索" />
        </button>
        <input
          type="text"
          class="searchTXT"
          placeholder="搜索城市(按回车进行搜索)"
          v-model="city"
          @keyup.enter="search"
          ref="input"
        />
      </div>
    </div>
    <div class="tableDiv">
      <div class="tableEDiv">
        <OrderByCity @open="open" :city="resCity" :items="items"></OrderByCity>
      </div>
    </div>
  </div>
</template>

<script>
import OrderByCity from "@/components/OrderByCity.vue";
export default {
  data() {
    return {
      back: "Square",
      city: "",
      resCity: "",
      items: [],
    };
  },
  components: {
    OrderByCity,
  },
  methods: {
    beBack() {
      this.$emit("pageChange", this.back);
    },
    search() {
      if (this.city == "") return;
      var axios = require("axios");
      this.resCity = this.city;
      var vue = this;

      var config = {
        method: "get",
        url: "/api/getBottleByCity",
        params: {
          city: vue.city,
        },
      };

      axios(config)
        .then(function (response) {
          console.log(JSON.stringify(response.data));
          vue.items = response.data.items;
          vue.city = "";
          vue.$refs.input.blur();
        })
        .catch(function (error) {
          console.log(error);
        });
    },
    open(uid) {
      this.$emit("open", uid);
    },
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
  margin-top: 10vh;
  background-image: url("@/images/backgroundSearch.png");
  background-repeat: no-repeat;
  background-size: 100% 100%;
  border-radius: 1rem;
}

button.searchBtn {
  background-color: transparent;
  text-align: left;
  width: 3em;
  height: 3em;
  border-width: 0px;
  margin: 0px;
  padding: auto;
  img {
    width: 90%;
  }
}
img.searchIco {
  width: 100%;
  height: 100%;
}
input.searchTXT {
  font-size: 110%;
  background-color: transparent;
  text-align: left;
  width: 80%;
  height: inherit;
  border: none;
  outline: none;
  margin-left: 2%;
  margin-right: 0px;
  margin-top: 0px;
  margin-bottom: 0px;
  padding: 0px;
}
input:-internal-autofill-previewed,
input:-internal-autofill-selected {
  transition: background-color 5000s ease-in-out 0s !important;
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
</style>