<template>
  <div class="all">
    <div class="top">
      <img src="@/images/topOnLoading.png" alt="" class="up" @load="start" />
      <img src="@/images/scut.png" alt="" class="logo" />
      <img src="@/images/word.png" alt="" class="Word" />
    </div>
    <div class="jdt">
      <img src="@/images/ligu.png" alt="" class="ligu" ref="ligu" />
      <div class="up" ref="up"></div>
    </div>
    <div class="word">漂流瓶正在被湖风吹来啦…</div>
    <!-- <button @click="change">next</button> -->
  </div>
</template>

<script>
export default {
  data() {
    return {
      next: "Square",
      jdt: 0,
      speed: 1,
    };
  },
  methods: {
    change() {
      this.$emit("pageChange", this.next);
    },
    start() {
      var id = setInterval(() => {
        if (this.jdt >= 100) {
          clearInterval(id);
          this.change();
        } else {
          this.jdt += this.speed;
          this.speed += 0.5;
          this.$refs.up.style.width = this.jdt + "%";
          this.$refs.ligu.style.left = this.jdt - 15 + "%";
        }
      }, 100);
    },
  },
  mounted() {},
};
</script>

<style lang="less" scoped>
.all {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 100%;

  .top {
    margin-top: 5vw;
    padding: 5vw;
    position: relative;
    display: flex;
    flex-direction: column;
    width: 80%;
    background-color: rgba(254, 247, 239, 1);
    .logo {
      position: absolute;
      width: 30vw;
      bottom: 20vw;
      right: 3vw;
    }
    .Word {
      margin-top: 5vw;
      width: 66vw;
    }
  }
  .jdt {
    position: relative;
    height: 5vw;
    width: 85%;
    margin-top: 18vw;
    background-color: rgba(229, 229, 229, 1);
    border-radius: 5vw;
    .ligu {
      position: absolute;
      width: 15vw;
      left: 0;
      top: -100%;
    }

    .up {
      background-color: rgba(44, 113, 134, 1);
      width: 0;
      height: 100%;
      border-radius: 5vw;
    }
  }
  .word {
    font-family: AlibabaPuHuiTi-3-65-Medium;
    margin-top: 5vw;
    font-size: 1.2rem;
  }
}
</style>