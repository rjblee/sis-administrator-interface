<template>
  <div>
    <label for="datepicker"><slot></slot> date</label>
    <b-form-datepicker
      v-model="selectedDate"
      class="mb-2 date-box"
      :max="maxDate"
      @input="updatedDate"
    ></b-form-datepicker>
  </div>
</template>

<script>
export default {
  name: "DatePicker",
  props: ["selected", "maxDate"],
  data() {
    return {
      selectedDate: this.selected,
    };
  },
  methods: {
    updatedDate() {
      this.$emit("updatedDate", this.selectedDate);
    },
  },
};
</script>

<style scoped>
.date-box {
  font-size: 0.8vw;
}
</style>
