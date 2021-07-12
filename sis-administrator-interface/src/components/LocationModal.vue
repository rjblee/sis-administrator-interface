<template>
  <div>
    <!-- <b-button v-b-modal.modal-scrollable><slot></slot></b-button> -->
    <slot />
    <b-modal id="location-modal" scrollable title="Select Locations">
      <b-form-checkbox id="checkbox-1" name="checkbox-1" value="accepted">
        Alberta
      </b-form-checkbox>
      <b-form-checkbox id="checkbox-2" name="checkbox-2" value="accepted">
        British Columbia
      </b-form-checkbox>
      <b-form-checkbox id="checkbox-3" name="checkbox-3" value="accepted">
        Manitoba
      </b-form-checkbox>
      <b-form-checkbox id="checkbox-4" name="checkbox-4" value="accepted">
        New Brunswick
      </b-form-checkbox>
      <b-form-checkbox id="checkbox-5" name="checkbox-5" value="accepted">
        New Foundland and Labrador
      </b-form-checkbox>
      <b-form-checkbox id="checkbox-6" name="checkbox-6" value="accepted">
        Nova Scotia
      </b-form-checkbox>
      <b-form-checkbox id="checkbox-6" name="checkbox-6" value="accepted">
        Ontario
      </b-form-checkbox>
      <b-form-checkbox id="checkbox-6" name="checkbox-6" value="accepted">
        Prince Edward Island
      </b-form-checkbox>
      <b-form-checkbox id="checkbox-6" name="checkbox-6" value="accepted">
        Quebec
      </b-form-checkbox>
      <b-form-checkbox id="checkbox-6" name="checkbox-6" value="accepted">
        Saskatchewan
      </b-form-checkbox>
    </b-modal>
  </div>
</template>

<script>
export default {
  name: "LocationModal",
};
</script>

<style scoped></style>
