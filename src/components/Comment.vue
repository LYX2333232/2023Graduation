<template>
  <div class="c1">
    <div class="lay1">
      <img :src="img" />
      <p>
        <b>{{ username }}</b>
      </p>
      <p class="t2">{{ t }}</p>
    </div>
    <div class="lay2">
      <p class="pp">
        <b>{{ Comment }}</b>
      </p>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    img: {},
    username: {},
    time: {}, //记录为一个字符串
    Comment: {},
  },
  computed: {
    t() {
      const now = new Date();
      const time = new Date(this.time);
      if (time.getMonth() < now.getMonth())
        return now.getMonth() - time.getMonth() + "月前";
      else if (time.getDate() < now.getDate())
        return now.getDate() - time.getDate() + "天前";
      else if (time.getHours() < now.getHours())
        return now.getHours() - time.getHours() + "小时前";
      else if (time.getMinutes() < now.getMinutes())
        return now.getMinutes() - time.getMinutes() + "分钟前";
      else return "刚刚";
    },
  },
};
</script>
<style lang="less" scoped>
.c1 {
  width: 100%;
  margin-top: 5vh;
  /* align-items: center; */
}

.lay1 {
  width: 100%;
  height: 50%;
  display: flex;
  align-items: center;
  img {
    width: 10%;
  }
  p {
    margin-left: 5vw;
  }
}

.lay2 {
  width: 100%;
  height: 50%;
  margin-top: 2vh;
}

.t2 {
  position: absolute;
  right: 10vw;
  color: rgb(44, 113, 134);
}

.pp {
  margin: 0;
  text-align: left;
  word-wrap: break-word;
  width: 95%;
}
</style>