<template>
  <div id="app">
    <Navbar>Course Reports</Navbar>
    <SideMenu></SideMenu>
    <div>
      <div>
        <b-form-group
          label="Course"
          label-for="table-style-variant"
          label-cols-md="2"
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
          label-cols-md="2"
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
      </div>

      <b-table
        outlined
        head-variant="light"
        hover
        :items="items"
        :fields="fields"
      >
        <template #cell(index)="data">
          {{ data.index + 1 }}
        </template>
        <template #cell(name)="data">
          <b class="text-info">{{ data.value.last }}</b
          >, <b>{{ data.value.first }}</b>
        </template>
      </b-table>
    </div>
  </div>
</template>

<script>
import Navbar from "./components/Navbar.vue";
import SideMenu from "./components/SideMenu.vue";

export default {
  name: "App",
  components: {
    Navbar,
    SideMenu,
  },
  data() {
    return {
      fields: [
        // { key: "Course", sortable: true },
        "index",
        // "name",
        { key: "name", label: "Full Name", sortable: true },
        "age",
        "school",
        "course",
        "group",
        "division",
      ],
      items: [
        {
          name: { first: "Tommy", last: "Miller" },
          age: 30,
          school: "Renert School",
          course: "BEAR SAFETY AWARENESS - SK",
        },
        {
          name: { first: "Lauren", last: "Braun" },
          age: 25,
          school: "Renert School",
          course: "ASBESTOS AWARENESS- BC",
        },
        {
          name: { first: "Jack", last: "Macdonald" },
          age: 20,
          school: "Renert School",
          course: "ALBERTA DRIVER HOURS OF SERVICE LIMITS
",
        },
        {
          name: { first: "Larsen", last: "Shaw" },
          age: 21,
          school: "Old Scona School",
          course: "BULLYING PREVENTION
",
        },
        {
          name: { first: "Geneva", last: "Wilson" },
          age: 18,
          school: "Webber Academy",
          course: "COLD STRESS AWARENESS",
        },
        {
          name: { first: "Jami", last: "Carney" },
          age: 24,
          school: "Fraser Valley High",
          course: "EMERGENCY RESPONSE PLANNING (ERP)",
        },
      ],
    };
  },
};
</script>

<style>
#app {
  /* font-family: Avenir, Helvetica, Arial, sans-serif; */
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /* text-align: center; */
  color: #2c3e50;
  /* margin-top: 60px; */
}
</style>
