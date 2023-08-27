<template>
  <div>
    <h2>{{ city }}</h2>
    <Bottle
      v-for="(item, index) in items"
      :key="index"
      :uid="item.uid"
      :img="item.img"
      :city="item.city"
      :time="item.time"
      :userName="item.username"
      :message="item.message"
      @open="open"
    ></Bottle>
  </div>
</template>
<script>
import Bottle from "@/components/Bottle.vue";
export default {
  props: {
    city: {
      require: true,
      type: String,
    },
    items: {},
  },
  components: {
    Bottle,
  },
  methods: {
    open(uid) {
      this.$emit("open", uid);
    },
  },
};
</script>