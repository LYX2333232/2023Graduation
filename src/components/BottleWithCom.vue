<template>
  <div class="toast">
    '<button class="botBtn" @click="open">
      <div class="bottDiv">
        <img class="tblImg" :src="img" alt="漂流瓶" />
        <div class="tblFrm">
          <div class="tblFrmIn">
            <h3 class="tblName">{{ userName }}</h3>
            <h4 class="tblTime">{{ time }}</h4>
          </div>
          <h4 class="tblInc">{{ message }}</h4>
        </div>
      </div>
      <CommentOnBottle
        v-for="(item, index) in myComments"
        :key="index"
        :myComment="item"
      ></CommentOnBottle>
    </button>
  </div>
</template>

<script>
import CommentOnBottle from "@/components/CommentOnBottle.vue";
export default {
  name: "Bottle",
  components: {
    CommentOnBottle,
  },
  props: {
    uid: {
      require: true,
      default: "",
    },
    img: {
      default: "",
      type: String,
    },
    userName: {
      default: "",
      type: String,
    },
    time: {
      default: "",
      type: String,
    },
    message: {
      default: "",
      type: String,
    },
    myComments: {},
  },
  methods: {
    open() {
      this.$emit("open", this.uid);
    },
  },
};
</script>

<style lang="less" scoped>
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