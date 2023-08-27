<template>
  <div class="all">
    <button class="back" @click="goBack">&lt;</button>
    <div class="top">分享</div>
    <div class="main" ref="main">
      <img src="@/images/bot.png" alt="" class="image" />
      <div class="down">
        <h2 class="my">我的漂流瓶</h2>
        <div id="text">{{ message }}</div>
        <div class="bottom">
          <h3 class="from">来自:{{ city }}</h3>
          <div class="QR">
            <div class="us">
              <img src="@/images/us.png" alt="" />
              <br />
              关注公众号
            </div>
            <div class="this">
              <img src="@/images/this.png" alt="" />
              <br />
              玩转漂流瓶
            </div>
          </div>
        </div>
      </div>
    </div>
    <div class="word">长按保存图片</div>
    <img class="result" src="" alt="" ref="result" />
  </div>
</template>
<script>
import html from "html2canvas";
export default {
  data() {
    return {
      back: "Receive",
    };
  },
  props: {
    logo: {},
    city: {},
    message: {},
  },

  methods: {
    goBack() {
      this.$emit("pageChange", this.back);
    },
    save() {
      let url;
      html(this.$refs.main, {
        useCORS: true,
        scale: 1,
        dpi: 50,
      }).then((canvas) => {
        // console.log(canvas);
        url = canvas.toDataURL();
        console.log(url);
        this.$refs.result.src = url;
      });
    },
  },
  mounted() {
    this.save();
  },
};
</script>
<style lang="less" scoped>
.all {
  display: flex;
  flex-direction: column;
  align-items: center;
}
.back {
  position: absolute;
  left: 2vw;
  top: 3vw;
  border: none;
  background-color: white;
  color: rgb(44, 113, 134);
  font-size: 3.5rem;
}
.top {
  background-color: transparent;
  font-size: 2rem;
  margin-top: 5vw;
  color: rgba(44, 113, 134, 1);
}
.main {
  position: relative;
  text-align: center;
  width: 85%;
  display: flex;
  flex-direction: column;
  align-items: center;
  .down {
    width: 100%;
    border-radius: 5vw;
    margin-top: 15vw;
    background-color: rgba(242, 238, 202, 1);
    .my {
      text-align: left;
      margin: 20vw 7vw 2vw;
    }
    #text {
      width: 70vw;
      height: 60vw;
      background-color: transparent;
      color: gray;
      border: none;
      font-size: 1rem;
      margin: 0 auto;
      text-align: left;
    }
    .bottom {
      width: 88%;
      display: flex;
      flex-direction: row;
      justify-content: space-between;
      margin: 10vw 8vw;
      .from {
        text-align: left;
        margin-top: 0;
        font-size: 1rem;
      }
      .QR {
        display: flex;
        justify-content: space-around;
        height: 30vw;
        border-radius: 5vw;
        padding: 3vw;
        background-color: rgba(44, 113, 134, 1);
        font-size: 0.8rem;
        .us {
          color: rgba(255, 255, 255, 1);
          img {
            width: 20vw;
          }
        }
        .this {
          color: rgba(255, 255, 255, 1);
          margin-left: 2vw;
          img {
            width: 22vw;
          }
        }
      }
    }
  }
  .image {
    position: absolute;
    width: 20%;
    left: 5vw;
    top: 10vw;
  }
}
.result {
  position: absolute;
  top: 20vw;
}
.word {
  margin-top: 10vw;
}
</style>