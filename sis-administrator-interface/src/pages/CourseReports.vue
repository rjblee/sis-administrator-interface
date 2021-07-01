<template>
  <div>
    <Navbar>Course Reports</Navbar>
    <SideMenu></SideMenu>
    <h2>This is Course Reports page</h2>
  </div>
</template>

<script>
import Navbar from "../components/Navbar.vue";
import SideMenu from "../components/SideMenu.vue";

export default {
  name: "CourseReports",
  components: {
    Navbar,
    SideMenu,
  },
};
</script>

<style></style>
