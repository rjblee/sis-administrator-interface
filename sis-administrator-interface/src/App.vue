<template>
  <div>
    <Navbar>Course Reports</Navbar>
    <SideMenu></SideMenu>

    <!-- --------Search bar-------- -->
    <!-- <b-col lg="6" class="my-1">
      <b-form-group
        label="Search"
        label-for="filter-input"
        label-cols-sm="3"
        label-align-sm="right"
        label-size="sm"
        class="mb-0"
      >
        <b-input-group size="sm">
          <b-form-input
            id="filter-input"
            v-model="filter"
            type="search"
            placeholder="Type to Search"
          ></b-form-input>

          <b-input-group-append>
            <b-button :disabled="!filter" @click="filter = ''">Clear</b-button>
          </b-input-group-append>
        </b-input-group>
      </b-form-group>
    </b-col> -->

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

      <b-container fluid id="app">
        <!-- User Interface controls -->
        <b-row>
          <b-col lg="6" class="my-1">
            <b-form-group
              label="Search"
              label-for="filter-input"
              label-cols-sm="1"
              label-align-sm="right"
              label-size="sm"
              class="mb-0"
            >
              <b-input-group size="sm">
                <b-form-input
                  id="filter-input"
                  v-model="filter"
                  type="search"
                  placeholder="Type to Search"
                ></b-form-input>

                <b-input-group-append>
                  <b-button :disabled="!filter" @click="filter = ''"
                    >Clear</b-button
                  >
                </b-input-group-append>
              </b-input-group>
            </b-form-group>
          </b-col>

          <b-col sm="7" md="6" class="my-1">
            <b-pagination
              v-model="currentPage"
              :total-rows="totalRows"
              :per-page="perPage"
              align="right"
              size="sm"
              class="my-0"
            ></b-pagination>
          </b-col>
        </b-row>

        <!-- Main table element -->
        <b-table
          :items="items"
          :fields="fields"
          :current-page="currentPage"
          :per-page="perPage"
          :filter="filter"
          :filter-included-fields="filterOn"
          :sort-by.sync="sortBy"
          :sort-desc.sync="sortDesc"
          :sort-direction="sortDirection"
          stacked="sm"
          show-empty
          head-variant="light"
          hover
          @filtered="onFiltered"
          id="table-transition-example"
          primary-key="index"
          :tbody-transition-props="transProps"
        >
          <template #cell(name)="row">
            {{ row.value.first }} {{ row.value.last }}
          </template>

          <!-- Action buttons for modal and details -->
          <!-- <template #cell(actions)="row">
          <b-button
            size="sm"
            @click="info(row.item, row.index, $event.target)"
            class="mr-1"
          >
            Info modal
          </b-button>
          <b-button size="sm" @click="row.toggleDetails">
            {{ row.detailsShowing ? "Hide" : "Show" }} Details
          </b-button>
        </template> -->

          <template #row-details="row">
            <b-card>
              <ul>
                <li v-for="(value, key) in row.item" :key="key">
                  {{ key }}: {{ value }}
                </li>
              </ul>
            </b-card>
          </template>
        </b-table>

        <!-- Info modal -->
        <b-modal
          :id="infoModal.id"
          :title="infoModal.title"
          ok-only
          @hide="resetInfoModal"
        >
          <pre>{{ infoModal.content }}</pre>
        </b-modal>
      </b-container>

      <!-- <b-table
        id="table-transition-example"
        class="table"
        outlined
        head-variant="light"
        hover
        :items="items"
        :fields="fields"
        :filter="filter"
        primary-key="index"
        :tbody-transition-props="transProps"
      >
        <template #cell(name)="data">
          <b class="text-info">{{ data.value.last }}</b
          >, <b>{{ data.value.first }}</b>
        </template>
      </b-table> -->
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
      items: [
        {
          age: 40,
          name: { first: "Jackerson", last: "Macdonald" },
        },
        { age: 21, name: { first: "Larry", last: "Shall" } },
        {
          age: 9,
          name: { first: "Mini", last: "Navarro" },
        },
        { age: 89, name: { first: "Geneva", last: "Wilson" } },
        { age: 38, name: { first: "Jamie", last: "Carson" } },
        { age: 27, name: { first: "Essie", last: "Dunlap" } },
        { age: 40, name: { first: "Thor", last: "Macdonald" } },
        {
          age: 87,
          name: { first: "Larsen", last: "Shaw" },
        },
        { age: 26, name: { first: "Mitzi", last: "Navarro" } },
        {
          age: 22,
          name: { first: "Genevieve", last: "Wilson" },
        },
        { age: 38, name: { first: "John", last: "Carney" } },
        { age: 29, name: { first: "Rick", last: "Dunlap" } },
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
          name: { first: "Julia", last: "Vanderwal" },
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
      fields: [
        {
          key: "name",
          label: "Full name",
          sortable: true,
          sortDirection: "desc",
        },
        {
          key: "age",
          sortable: true,
        },
        {
          key: "school",
          sortable: true,
          sortByFormatted: true,
          filterByFormatted: true,
        },
        { key: "course", sortable: true },
      ],
      totalRows: 1,
      currentPage: 1,
      perPage: 10,
      sortBy: "",
      sortDesc: false,
      sortDirection: "asc",
      filter: null,
      filterOn: [],
      infoModal: {
        id: "info-modal",
        title: "",
        content: "",
      },
    };
  },
  computed: {
    sortOptions() {
      // Create an options list from our fields
      return this.fields
        .filter((f) => f.sortable)
        .map((f) => {
          return { text: f.label, value: f.key };
        });
    },
  },
  mounted() {
    // Set the initial number of items
    this.totalRows = this.items.length;
  },
  methods: {
    info(item, index, button) {
      this.infoModal.title = `Row index: ${index}`;
      this.infoModal.content = JSON.stringify(item, null, 2);
      this.$root.$emit("bv::show::modal", this.infoModal.id, button);
    },
    resetInfoModal() {
      this.infoModal.title = "";
      this.infoModal.content = "";
    },
    onFiltered(filteredItems) {
      // Trigger pagination to update the number of buttons/pages due to filtering
      this.totalRows = filteredItems.length;
      this.currentPage = 1;
    },
  },
};

// ---------------------------------------------
// export default {
//   name: "App",
//   components: {
//     Navbar,
//     SideMenu,
//   },
//   data() {
//     return {
//       transProps: {
//         // Transition name
//         name: "flip-list",
//       },
//       fields: [
//         { key: "name", label: "Full Name", sortable: true },

//         "age",
//         "school",
//         "course",
//         "group",
//         "division",
//       ],
//       items: [
//         {
//           name: { first: "Tommy", last: "Miller" },
//           age: 30,
//           school: "Alberta High School of Fine Arts",
//           course: "BEAR SAFETY AWARENESS - SK",
//         },
//         {
//           name: { first: "Lauren", last: "Braun" },
//           age: 25,
//           school: "Renert School",
//           course: "ASBESTOS AWARENESS- BC",
//         },
//         {
//           name: { first: "Jack", last: "Macdonald" },
//           age: 20,
//           school: "Airdrie Koinonia Christian School",
//           course: "ALBERTA DRIVER HOURS OF SERVICE LIMITS",
//         },
//         {
//           name: { first: "Larsen", last: "Shaw" },
//           age: 21,
//           school: "Old Scona School",
//           course: "BULLYING PREVENTION",
//         },
//         {
//           name: { first: "Geneva", last: "Wilson" },
//           age: 18,
//           school: "Webber Academy",
//           course: "COLD STRESS AWARENESS",
//         },
//         {
//           name: { first: "Jami", last: "Carney" },
//           age: 24,
//           school: "Fraser Valley High",
//           course: "EMERGENCY RESPONSE PLANNING (ERP)",
//         },
//       ],
//       filter: null,
//     };
//   },
// };
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
  margin: 10px;
  padding: 10px;
}

.table {
  margin: 10px;
}

table#table-transition-example .flip-list-move {
  transition: transform 1s;
}
</style>
