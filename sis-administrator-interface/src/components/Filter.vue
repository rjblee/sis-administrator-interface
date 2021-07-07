<template>
  <div class="filter">
    <h4>Filter By</h4>
    <b-form-group
      label="Course"
      label-for="table-style-variant"
      label-cols-md="4"
    >
      <b-form-select
        id="table-style-variant"
        v-model="tableVariant"
        :options="tableVariants"
      >
        <template #first>
          <option value="">-- None --</option>
        </template>
      </b-form-select>
    </b-form-group>

    <b-form-group
      label="Group"
      label-for="table-style-variant"
      label-cols-md="4"
    >
      <b-form-select
        id="table-style-variant"
        v-model="tableVariant"
        :options="tableVariants"
      >
        <template #first>
          <option value="">-- None --</option>
        </template>
      </b-form-select>
    </b-form-group>

    <b-form-group
      label="Location"
      label-for="table-style-variant"
      label-cols-md="4"
    >
      <b-form-select
        id="table-style-variant"
        v-model="tableVariant"
        :options="tableVariants"
      >
        <template #first>
          <option value="">-- None --</option>
        </template>
      </b-form-select>
    </b-form-group>

    <b-form-group
      label="Date"
      label-for="table-style-variant"
      label-cols-md="4"
    >
      <b-form-select
        id="table-style-variant"
        v-model="tableVariant"
        :options="tableVariants"
      >
        <template #first>
          <option value="">-- None --</option>
          <option value="">Last day</option>
          <option value="">Last week</option>
          <option value="">Last month</option>
        </template>
      </b-form-select>
    </b-form-group>
  </div>
</template>

<script>
export default {
  name: "Filter",
};
</script>

<style scoped>
.filter {
  border: 1px solid black;
  width: 20%;
  margin: 10px;
  padding: 10px;
}
</style>
