<template>
  <h4 class="simple-data">Total {{ specificName }}: {{ totalNumber }}</h4>
</template>

<script>
export default {
  name: "SimpleData",
  props: ["specificName", "totalNumber"],
  data() {
    return {};
  },
};
</script>

<style scoped>
.simple-data {
  /* border: 1px solid black; */
  width: 100%;
  margin: 10px;
  padding: 10px;
}
</style>
