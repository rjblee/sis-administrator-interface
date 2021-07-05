<template>
  <div id="app">
    <Navbar>Course Reports</Navbar>
    <SideMenu></SideMenu>

    <div class="main-content">
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

      <b-table
        id="table-transition-example"
        outlined
        head-variant="light"
        hover
        :items="items"
        :fields="fields"
        primary-key="index"
        :tbody-transition-props="transProps"
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
      transProps: {
        // Transition name
        name: "flip-list",
      },
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
          school: "Alberta High School of Fine Arts",
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
          school: "Airdrie Koinonia Christian School",
          course: "ALBERTA DRIVER HOURS OF SERVICE LIMITS",
        },
        {
          name: { first: "Larsen", last: "Shaw" },
          age: 21,
          school: "Old Scona School",
          course: "BULLYING PREVENTION",
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

.main-content {
  display: flex;
}

.filter {
  border: 1px solid black;
  width: 20%;
}

table#table-transition-example .flip-list-move {
  transition: transform 1s;
}
</style>
