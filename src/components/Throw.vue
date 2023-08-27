<template>
  <div class="all">
    <img src="@/images/ThrowBackground.png" alt="" class="bcc" @load="Throw" />
    <img src="@/images/ThrowBottle.png" alt="" class="bot" ref="img" />
  </div>
</template>

<script>
export default {
  data() {
    return {
      back: "Square",
      left: 2,
      bottom: 50,
    };
  },
  methods: {
    Throw() {
      let t = 0;
      let vx = 1,
        vy = 2,
        ya = -0.08;
      var id = setInterval(() => {
        if (t >= 60) {
          clearInterval(id);
          this.$emit("pageChange", this.back);
        }
        if (t < 46) {
          this.left += vx;
          this.bottom += vy;
          vy += ya;
          this.$refs.img.style.left = this.left + "vw";
          this.$refs.img.style.bottom = this.bottom + "vh";
        }
        t++;
      }, 50);
    },
  },
};
</script>

<style lang="less" scoped>
.all {
  position: relative;
  width: 90vw;
  height: 100vh;
  margin: 30% auto;
  .bcc {
    width: 100%;
  }
  .bot {
    position: absolute;
    width: 20%;
    left: 2vw;
    bottom: 50vh;
  }
}
</style>