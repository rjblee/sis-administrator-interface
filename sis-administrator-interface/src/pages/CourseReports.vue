<template>
  <div>
    <Navbar>Course Reports</Navbar>
    <SideMenu></SideMenu>

    <div class="main-content">
      <b-col md="2">
        <SimpleData
          specificName="Certifications"
          :totalNumber="certificateNumber"
        ></SimpleData>
        <div class="filter">
          <h4>Filter</h4>

          <CourseModal
            :ident="'CourseC'"
            @courseSelectorClosed="getCourses"
            class="modal-button"
          >
            <b-button variant="primary" v-b-modal="'CourseC'" block
              >Course</b-button
            >
          </CourseModal>
          <GroupModal
            :ident="'CourseG'"
            @groupSelectorClosed="getGroups"
            class="modal-button"
            ><b-button variant="primary" v-b-modal="'CourseG'" block
              >Group</b-button
            ></GroupModal
          >
          <LocationModal class="modal-button" v-on:childToParent="">
            <b-button variant="primary" v-b-modal.location-modal block
              >Location</b-button
            >
            <span>{{ selectedLocations }}</span>
          </LocationModal>
          <SchoolModal class="modal-button">
            <b-button variant="primary" v-b-modal.school-modal block
              >School</b-button
            >
          </SchoolModal>

          <div>
            <DatePicker
              :selected="selectedStart"
              @updatedDate="updateStartDate"
              :maxDate="maxStartDate"
              >Starting</DatePicker
            >
            <DatePicker
              :selected="selectedEnd"
              @updatedDate="updateEndDate"
              :maxDate="maxEndDate"
              >Ending</DatePicker
            >
          </div>
          <hr />
          <b-button variant="primary" @click="submitForm">Apply</b-button>
        </div>
      </b-col>

      <b-container fluid id="app">
        <!-- User Interface controls -->
        <b-row style="justify-content: space-between">
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
                  <b-button
                    class="clear-button"
                    :disabled="!filter"
                    @click="filter = ''"
                    >Clear</b-button
                  >
                </b-input-group-append>
              </b-input-group>
            </b-form-group>
          </b-col>
          <NewAdminModal class="">
            <b-button variant="primary" v-b-modal.admin-modal
              >New Admin</b-button
            >
          </NewAdminModal>
          <b-col sm="12" md="3" class="my-1">
            <b-form-group
              label=""
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
            <download-excel
              :name="name"
              :header="fileHeader"
              :fields="excelFields"
              :data="reportResults"
            >
              <b-button type="button" class="fileNameInput"
                >Download Excel</b-button
              >
            </download-excel>
          </div>
        </b-row>

        <!-- Main table element -->
        <div ref="export_table">
          <b-table
            v-if="!tableLoading"
            :items="reportResults"
            :current-page="currentPage"
            :per-page="perPage"
            :fields="fields"
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
            class="course-reports-table"
          >
            <template #cell(name)="row">
              {{ row.value.first }} {{ row.value.last }}
            </template>

            <template #row-details="row">
              <b-card>
                <ul>
                  <li v-for="(value, key) in row.item" :key="key">
                    {{ key }}: {{ value }}
                  </li>
                </ul>
              </b-card>
            </template>

            <template #cell(details)="row">
              <!-- Delete button for Registration page -->
              <!-- <b-button size="sm" @click="deleteStudent(row.index)" class="mr-1">
              Delete
            </b-button> -->
              <b-button size="sm" @click="row.toggleDetails">
                {{ row.detailsShowing ? "Hide" : "Show" }} Details
              </b-button>
            </template>
          </b-table>
          <div v-else class=" text-success ">
            <b-spinner class=""></b-spinner>
            <strong class="">Generating Report</strong>
          </div>
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
    </div>
  </div>
</template>
<script src="https://cdnjs.cloudflare.com/ajax/libs/lodash.js/4.17.4/lodash.min.js"></script>
<script>
import Navbar from "../components/Navbar.vue";
import SideMenu from "../components/SideMenu.vue";
import Filter from "../components/Filter.vue";
import CourseModal from "../components/CourseModal.vue";
import GroupModal from "../components/GroupModal.vue";
import LocationModal from "../components/LocationModal.vue";
import SchoolModal from "../components/SchoolModal.vue";
import NewAdminModal from "../components/NewAdminModal.vue";
import DatePicker from "../components/DatePicker.vue";
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
    NewAdminModal,
    DatePicker,
    SimpleData,
  },

  data() {
    return {
      transProps: {
        // Transition name
        name: "flip-list",
      },
      totalRows: 1,
      currentPage: 1,
      perPage: 10,
      pageOptions: [
        { value: 5, text: "5 per page" },
        { value: 10, text: "10 per page" },
        { value: 15, text: "15 per page" },
        { value: 20, text: "20 per page" },
        { value: 100, text: "100 per page" },
      ],
      excelFields: {},
      sortBy: "",
      sortDesc: false,
      sortDirection: "asc",
      filter: "",
      filterOn: [],
      groupsSelected: [],
      coursesSelected: [],
      reportResults: [],
      name: "",
      selectedStart: "",
      selectedEnd: "",
      maxStartDate: "",
      maxEndDate: "",
      fields: [],
      certificateNumber: 0,
      tableLoading: false,
      showCourse: false,
      Course: "Course",
      fileHeader: "",
      infoModal: {
        id: "info-modal",
        title: "",
        content: "",
      },
      selectedLocations: "",
    };
  },
  created() {
    //!The to and from dates are not currently working for report type 11
    this.maxEndDate = formatDate(new Date());
    this.selectedEnd = this.maxEndDate;
    var startdate = new Date();
    startdate.setMonth(startdate.getMonth() - 10);
    this.selectedStart = formatDate(startdate);
    this.reportResults = [];
    let reportObj = {
      groupID: 1,
      reportType: 10,
      includeContent: "*",
      includeGroups: "*",
      includeUsers: "*",
      startDate: dayOfYear(formatDate(startdate)),
      endDate: dayOfYear(this.maxEndDate),
      /*  startDate:"",
      endDate:"",*/
    };
    this.generateReport(reportObj); //call the generate report function in order to load in the initial data
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
  methods: {
    generateReport(reportObj) {
      this.tableLoading = true;
      let starttime = new Date();
      this.$store.dispatch("generateReport", reportObj).then((ret) => {
        console.log("Time to generate a report", new Date() - starttime);
        console.log("Finished generating the report", ret);
        this.fileHeader =
          ret.generalInformation +
          "\n" +
          ret.groupsIncluded +
          "\n" +
          ret.dateRange;
        console.log(this.fileHeader);
        if (reportObj.reportType == 11) {
          for (var i = 0; i < ret.results.length; i++) {
            let newResults = {};

            //newResults.name=ret.results[i].Firstname+" "+ret.results[i].Lastname;
            (newResults.name = {
              first: ret.results[i].Firstname,
              last: ret.results[i].Lastname,
            }),
              //console.log(newResults.name);
              (newResults.Certification = ret.results[i].Certification);
            if (ret.results[i].DateCertified == "") {
              newResults.Certified = false;
              newResults.DateCertified = ret.results[i].DateCertified;
            } else {
              newResults.Certified = true;
              newResults.DateCertified = ret.results[i].DateCertified;
            }
            newResults.MasterOnce = ret.results[i].MasterOnce;
            newResults.ExpiryDate = ret.results[i].ExpiryDate;
            this.reportResults.push(newResults);
            this.sortBy = "Certification";
            this.fields = [
              {
                key: "Certification",
                sortable: true,
              },
              {
                key: "name",
                sortable: true,
              },
              {
                key: "DateCertified",
                sortable: true,
              },
              {
                key: "DateMastered",
                sortable: true,
              },
              /*{
            key: "Company",
            sortable: true,
          },*/
            ];
          }
        } else if (reportObj.reportType == 10) {
          for (var a = 0; a < ret.results.length; a++) {
            let newResults = {};
            newResults.Content = ret.results[a].Content;
            (newResults.name = {
              first: ret.results[a].FirstName,
              last: ret.results[a].LastName,
            }),
              (newResults.DateCertified = ret.results[a].Date);
            newResults.Mastered = ret.results[a].Mastered;
            newResults.LoginID = ret.results[a].LoginID;
            newResults.Company = ret.results[a].Company;
            this.reportResults.push(newResults);
          }
          this.sortBy = "Content";
          this.fields = [
            {
              key: "Content",
              sortable: true,
            },
            {
              key: "name",
              sortable: true,
            },
            {
              key: "DateCertified",
              sortable: true,
            },
            {
              key: "Mastered",
              sortable: true,
            },
            {
              key: "Company",
              sortable: true,
            },
          ];
          this.excelFields = {
            "First Name": "name.first",
            "Last Name": "name.last",
            Content: "Content",
            "Date Mastered": "DateCertified",
            Mastered: "Mastered",
            LoginID: "LoginID",
            Company: "Company",
          };
        }
        this.certificateNumber = this.reportResults.length;
        this.totalRows = this.reportResults.length;
        this.tableLoading = false;
      });
    },
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
    getCourses(courses) {
      console.log("Courses returned by the Selector", courses);
      this.coursesSelected = courses;
    },
    getGroups(groups) {
      console.log("groups returned by the Selector", groups);
      this.groupsSelected = groups;
    },
    submitForm() {
      var courseIDs = "";
      var groupIDs = "";
      if (this.coursesSelected.length == 0) {
        //if there are no results assume that all fields are being selected
        courseIDs = "*";
      } else {
        for (var i = 0; i < this.coursesSelected.length; i++) {
          courseIDs += this.coursesSelected[i].contentID + ";";
        }
      }

      if (this.groupsSelected.length == 0) {
        groupIDs = "*";
      } else {
        for (var t = 0; t < this.groupsSelected.length; t++) {
          groupIDs += this.groupsSelected[t].groupID.toString() + ";";
        }
      }
      console.log("Course IDs", courseIDs);
      console.log("Group IDs", groupIDs);
      let reportObj = {
        groupID: 1,
        reportType: 10,
        includeContent: courseIDs,
        includeGroups: groupIDs,
        includeUsers: "*",
        startDate: dayOfYear(this.selectedStart),
        endDate: dayOfYear(this.selectedEnd),
        /*startDate:"",
      endDate:"",*/
      };
      this.reportResults = []; //set the report results to be an empty array in order to clear the results
      this.generateReport(reportObj);
    },
    updateStartDate(date) {
      this.selectedStart = date;
    },
    updateEndDate(date) {
      this.selectedEnd = date;
      this.maxStartDate = getDayBefore(date);
      getDayBefore(date);
    },
  },
};

function formatDate(date) {
  var d = new Date(date),
    month = "" + (d.getMonth() + 1),
    day = "" + d.getDate(),
    year = d.getFullYear();

  if (month.length < 2) month = "0" + month;
  if (day.length < 2) day = "0" + day;

  return [year, month, day].join("-");
}
function dayOfYear(date) {
  //this function will get the day of the current year and parse it into the format that we need
  date = date.split("-"); //split the passed in data that was chosen
  let startDate = new Date(); //create a new date object and pass in the values
  startDate.setFullYear(
    parseInt(date[0]),
    parseInt(date[1]) - 1,
    parseInt(date[2])
  ); //Months are zero indexed so this will take care of them
  var start = new Date(startDate.getFullYear(), 0, 0); //make a new date object to represent the first of the year
  var diff =
    startDate -
    start +
    (start.getTimezoneOffset() - startDate.getTimezoneOffset()) * 60 * 1000; //difference in seconds
  var oneDay = 1000 * 60 * 60 * 24;
  var day = Math.floor(diff / oneDay);
  return day.toString() + "," + startDate.getFullYear().toString();
}

function getDayBefore(date) {
  date = date.split("-"); //split the passed in data that was chosen
  let dayBefore = new Date(); //create a new date object and pass in the values
  dayBefore.setFullYear(
    parseInt(date[0]),
    parseInt(date[1]) - 1,
    parseInt(date[2])
  ); //Months are zero indexed so this will take care of them
  dayBefore.setDate(dayBefore.getDate() - 1); //find the day one day in the past
  return formatDate(dayBefore); //return the day in the proper format
}
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

.clear-button {
  background-color: #ff7e00;
  margin-left: 10px;
}

.fileName {
  display: flex;
  flex-direction: row;
  justify-content: flex-end;
  margin-right: 4vw;
}

.fileNameInput {
  width: 13vw;
  margin-left: 1vw;
  margin-right: 1vw;
}

.simple-data {
  /* border: 1px solid black; */
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
  border-radius: 10px;
  width: 100%;
  margin: 10px;
  padding: 10px;
}

.filter {
  display: flex;
  flex-direction: column;
  align-items: center;
  border-radius: 10px;
  width: 100%;
  margin: 10px;
  padding: 10px;
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
}

.course-reports-table {
  /* border: 1px solid black; */
  margin: 10px 0;
  border-radius: 20px;
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
}

.modal-button {
  width: 100%;
  padding: 10px;
  /* background-color: #1a65a4; */
}

table#table-transition-example .flip-list-move {
  transition: transform 1s;
}
</style>
