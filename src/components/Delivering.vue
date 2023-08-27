<template>
  <div>
    <div class="total">
      <button class="button1" @click="beBack">&lt;</button>
      <h4 class="text0">投送漂流瓶</h4>

      <div class="pic">
        <img src="@/images/b1.png" />
      </div>
      <div class="input-container">
        <div class="left">城市：</div>
        <input type="text" class="input-box" v-model="city" />
      </div>
      <div class="textlong">
        <textarea
          class="transparent-textarea"
          cols="35"
          rows="10"
          v-model="output"
          placeholder="分享自己对毕业后前往的城市/家乡
城市/向往城市想说的话吧…
或许你可以说出自己的担忧，
说不定有人会为你解答哟~
(最多500个字哦)"
          maxlength="500"
        ></textarea>
      </div>
      <div class="button-container">
        <button class="button0" @click="goThrow">点击投送漂流瓶</button>
      </div>
    </div>
    <!-- <button @click="change">返回广场</button> -->
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      city: "",
      output: "",
      back: "Square",
      throw: "Throw",
    };
  },
  props: {
    mylogo: {},
    myName: {},
  },
  methods: {
    beBack() {
      this.$emit("pageChange", this.back);
    },
    goThrow() {
      if (this.city == "") {
        alert("你还没有选择城市哦！");
        return;
      }
      if (this.output == "") {
        alert("你还没有写好自己的漂流瓶吧！");
        return;
      }
      //上传数据
      const vue = this;
      const t = new Date();

      const config = {
        method: "post",
        url: "/api/delivering",
        // params是变成URL参数，data是放在请求体里面的
        params: {
          username: this.myName,
          img: this.mylogo,
          city: this.city,
          message: this.output,
          time: t.getFullYear() + "-" + (t.getMonth() + 1) + "-" + t.getDate(),
        },
      };

      axios(config).then((res) => {
        console.log("res", res);
        this.$emit("pageChange", this.throw);
      });
    },
  },
};
</script>

<style lang="less" scoped>
.transparent-textarea {
  background-color: transparent;
  color: gray;
  border: none;
  outline: none;
  width: 90%;
  height: 25vh;
  font-size: 1.1rem;
}
textarea::-webkit-input-placeholder {
  font-family: AliAlibabaPuHuiTi-3-65-Medium;
}

.total {
  display: flex;
  flex-direction: column;
  align-items: center;
  height: 100%;
}

.button0 {
  border: none;
  width: 80%;
  height: 25%;
  border-radius: 1vw;
  background-color: rgb(44, 113, 134);
  color: white;
}

.pic {
  width: 100%;
  height: 15vh;

  display: flex;
  justify-content: center;
  align-items: center;
}

.pic img {
  width: 30%;
  /* Adjust the width as needed */
  object-fit: cover;
  /* border-radius: 10%; */
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

.text0 {
  margin-top: 1.7rem;
  color: rgb(44, 113, 134);
  font-size: 2rem;
  font-weight: 530;
}

.input-container {
  width: 90%;
  background-image: url("@/images/text1.png");
  background-size: cover;
  background-repeat: no-repeat;
  display: flex;
  align-items: center;
  height: 5vh;
  border-radius: 1.2rem;
}
.left {
  margin-left: 8%;
  font-size: 1rem;
  font-weight: 1000;
}
.input-box {
  background: none;
  border: none;
  outline: none;
  font-size: 1rem;
  height: 80%;
  width: 67%;
  /* 适当调整输入框与文字的间距 */
}

.button-container {
  border: none;
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 20vh;
  button {
    border-radius: 1.2rem;
  }
}

.textlong {
  position: relative;
  background-image: url("@/images/ta.png");
  background-size: cover;
  background-repeat: no-repeat;
  margin-top: 5vh;
  width: 90%;
  height: 32vh;
  display: flex;
  border-radius: 2.5rem;
  justify-content: center;
  align-items: center;
}
</style>
