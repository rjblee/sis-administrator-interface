<template>
  <div>
    <b-navbar toggleable="lg" type="dark" variant="dark">
      <b-navbar-brand tag="h1" class="mb-0" href="#"
        >Administrator Dashboard - <slot></slot
      ></b-navbar-brand>

      <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

      <b-collapse id="nav-collapse" is-nav>
        <b-navbar-nav>
          <b-nav-item-dropdown right href="#">
            <template #button-content>
              Dashboard
            </template>
            <b-dropdown-item href="#">Home</b-dropdown-item>
          </b-nav-item-dropdown>
          <b-nav-item-dropdown href="#">
            <template #button-content>
              Reports
            </template>
            <b-dropdown-item href="#">Course Reports</b-dropdown-item>
            <b-dropdown-item href="#">Registration Reports</b-dropdown-item>
            <b-dropdown-item href="#">Contact Reports</b-dropdown-item>
          </b-nav-item-dropdown>
          <b-nav-item-dropdown href="#">
            <template #button-content>
              Registration
            </template>
            <b-dropdown-item href="#">Global Admin</b-dropdown-item>
            <b-dropdown-item href="#">Primary Admin</b-dropdown-item>
            <b-dropdown-item href="#">Teachers</b-dropdown-item>
            <b-dropdown-item href="#">Divisions</b-dropdown-item>
          </b-nav-item-dropdown>
          <b-nav-item-dropdown href="#">
            <template #button-content>
              Course Access
            </template>
            <b-dropdown-item href="#">Assign Access</b-dropdown-item>
          </b-nav-item-dropdown>
        </b-navbar-nav>

        <!-- Right aligned nav items -->
        <b-navbar-nav class="ml-auto">
          <b-nav-form>
            <b-button size="md" class="my-2 my-sm-0" type="submit"
              >New</b-button
            >
          </b-nav-form>

          <b-nav-item-dropdown right>
            <!-- Using 'button-content' slot -->
            <template #button-content>
              User
            </template>
            <b-dropdown-item href="#">Profile</b-dropdown-item>
            <b-dropdown-item href="#">Sign Out</b-dropdown-item>
          </b-nav-item-dropdown>
        </b-navbar-nav>
      </b-collapse>
    </b-navbar>
  </div>
</template>

<script>
export default {
  name: "Navbar",
  data() {
    return {};
  },
};
</script>

<style scoped></style>
