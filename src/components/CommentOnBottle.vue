<template>
  <button class="comBtn">
    <div class="comDiv">
      <h4 class="comTXT">{{ myComment }}</h4>
    </div>
  </button>
</template>
<script>
export default {
  props: {
    myComment: {},
  },
};
</script>

<style scoped>
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
div.comDiv {
  font-size: 110%;
  width: 94%;
  padding-left: 3%;
  padding-right: 3%;
  padding-top: 2%;
  padding-bottom: 2%;
  background-color: LightYellow;
  border-radius: 1em;
}
h4.comTXT {
  margin: 0px;
  width: 100%;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}
</style>