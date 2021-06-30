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
      ></b-table>
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
        "first_name",
        "last_name",
        "age",
        "school",
      ],
      items: [
        {
          first_name: "Dickerson",
          last_name: "Macdonald",
          age: 30,
          school: "Renert School",
        },
        {
          first_name: "Larsen",
          last_name: "Shaw",
          age: 21,
          school: "Old Scona School",
        },
        {
          first_name: "Geneva",
          last_name: "Wilson",
          age: 18,
          school: "Webber Academy",
        },
        {
          first_name: "Jami",
          last_name: "Carney",
          age: 24,
          school: "Fraser Valley High",
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
