<template>
  <div>
    <Navbar>Registration Reports</Navbar>
    <SideMenu></SideMenu>

    <div class="main-content">
      <b-col md="2">
        <h4 v-bind="items" class="simple-data">
          Total Registrations: {{ items.length }}
        </h4>
        <SimpleData
          specificName="Registrations"
          :totalNumber="items.length"
        ></SimpleData>
        <div class="filter">
          <h4>Filter By</h4>
          <CourseModal :ident="'RegistrationC'" class="modal-button">
            <b-button variant="primary" v-b-modal="'RegistrationC'"
              >Course</b-button
            >
          </CourseModal>
          <GroupModal class="modal-button"
            ><b-button variant="primary" v-b-modal.group-modal
              >Group</b-button
            ></GroupModal
          >
          <LocationModal class="modal-button">
            <b-button variant="primary" v-b-modal.location-modal
              >Location</b-button
            >
          </LocationModal>
          <SchoolModal class="modal-button">
            <b-button variant="primary" v-b-modal.school-modal>School</b-button>
          </SchoolModal>
        </div>
      </b-col>

      <Filter></Filter>

      <b-container fluid id="app">
        <!-- User Interface controls -->
        <b-row>
          <b-col md="6" lg="6" class="my-1">
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
          <b-col sm="12" md="6" class="my-1">
            <b-form-group
              label="Per page"
              label-for="per-page-select"
              label-cols-sm="6"
              label-cols-md="6"
              label-cols-lg="6"
              label-align-sm="right"
              label-size="sm"
              class="mb-0"
            >
              <b-form-select
                id="per-page-select"
                v-model="perPage"
                :options="pageOptions"
                size="sm"
              ></b-form-select>
            </b-form-group>
          </b-col>
        </b-row>

        <b-row class="fileName">
          <div>
            <input
              class="fileNameInput"
              type="text"
              v-model="name"
              placeholder="Save as Excel"
            />
            <b-button
              class="btn btn-fill"
              variant="primary"
              @click="exportExcel('xlsx')"
              >Download</b-button
            >
          </div>
        </b-row>

        <!-- Main table element -->
        <div ref="export_table">
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
            head-variant="dark"
            hover
            @filtered="onFiltered"
            id="table-transition-example"
            primary-key="index"
            :tbody-transition-props="transProps"
            class="registration-reports-table"
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

            <template #cell(delete)="row">
              <!-- Delete button for Registration page -->
              <b-button
                size="sm"
                @click="deleteStudent(row.index)"
                class="mr-1"
              >
                Delete
              </b-button>
              <!-- <b-button size="sm" @click="row.toggleDetails">
              {{ row.detailsShowing ? "Hide" : "Show" }} Details
            </b-button> -->
            </template>
          </b-table>
        </div>

        <!-- Info modal -->
        <!-- <b-modal
          :id="infoModal.id"
          :title="infoModal.title"
          ok-only
          @hide="resetInfoModal"
        >
          <pre>{{ infoModal.content }}</pre>
        </b-modal> -->

        <b-col sm="12" md="12" class="my-1">
          <b-pagination
            v-model="currentPage"
            :total-rows="totalRows"
            :per-page="perPage"
            align="right"
            size="sm"
            class="my-0"
          ></b-pagination>
        </b-col>
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
import Navbar from "../components/Navbar.vue";
import SideMenu from "../components/SideMenu.vue";
import Filter from "../components/Filter.vue";
import CourseModal from "../components/CourseModal.vue";
import GroupModal from "../components/GroupModal.vue";
import LocationModal from "../components/LocationModal.vue";
import SchoolModal from "../components/SchoolModal.vue";
import SimpleData from "../components/SimpleData.vue";
import XLSX from "xlsx";

export default {
  name: "App",
  components: {
    Navbar,
    SideMenu,
    Filter,
    CourseModal,
    GroupModal,
    LocationModal,
    SchoolModal,
    SimpleData,
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
        { key: "location", sortable: true },
        { key: "delete" },
      ],
      totalRows: 1,
      currentPage: 1,
      perPage: 10,
      pageOptions: [5, 10, 15, 20, { value: 100, text: "100" }],
      sortBy: "",
      sortDesc: false,
      sortDirection: "asc",
      filter: null,
      filterOn: [],
      Registration: "Registration",
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

    deleteStudent(i) {
      this.items.splice(i, 1);
    },

    exportExcel(type, fn, dl) {
      var elt = this.$refs.export_table;
      var wb = XLSX.utils.table_to_book(elt, { sheet: "Sheet JS" });
      return dl
        ? XLSX.write(wb, { bookType: type, bookSST: true, type: "base64" })
        : XLSX.writeFile(
            wb,
            fn || (this.name + "." || "SheetJSTableExport.") + "xlsx"
          );
    },
  },
};
</script>

<style scoped>
#app {
  /* font-family: Avenir, Helvetica, Arial, sans-serif; */
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /* text-align: center; */
  color: #2c3e50;
}

.main-content {
  display: flex;
  margin-top: 30px;
}

.simple-data {
  /* border: 1px solid black; */
  border-radius: 10px;
  width: 100%;
  margin: 10px;
  padding: 10px;
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
}

.filter {
  /* border: 1px solid black; */
  border-radius: 10px;
  width: 100%;
  margin: 10px;
  padding: 10px;
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
}

.registration-reports-table {
  margin: 10px 0;
  border-radius: 20px;
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
}

.modal-button {
  width: 100%;
  padding: 10px;
}

table#table-transition-example .flip-list-move {
  transition: transform 1s;
}
</style>
