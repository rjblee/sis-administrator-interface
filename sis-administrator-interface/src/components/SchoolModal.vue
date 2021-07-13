<template>
  <div>
    <!-- <b-button v-b-modal.modal-scrollable><slot></slot></b-button> -->
    <slot />
    <b-modal id="school-modal" scrollable title="Select Schools">
      <b-form-checkbox id="checkbox-6" name="checkbox-6" value="accepted">
        Airdrie Francophone School
      </b-form-checkbox>
      <b-form-checkbox id="checkbox-1" name="checkbox-1" value="accepted">
        Ashmont Secondary Community School
      </b-form-checkbox>
      <b-form-checkbox id="checkbox-2" name="checkbox-2" value="accepted">
        Banff Community High School
      </b-form-checkbox>
      <b-form-checkbox id="checkbox-3" name="checkbox-3" value="accepted">
        Drumheller Valley Secondary School
      </b-form-checkbox>
      <b-form-checkbox id="checkbox-4" name="checkbox-4" value="accepted">
        Eckville Junior Senior High School
      </b-form-checkbox>
      <b-form-checkbox id="checkbox-5" name="checkbox-5" value="accepted">
        St. John Paul II Catholic High School
      </b-form-checkbox>
      <b-form-checkbox id="checkbox-6" name="checkbox-6" value="accepted">
        Peace Academy of Virtual Education
      </b-form-checkbox>
      <b-form-checkbox id="checkbox-6" name="checkbox-6" value="accepted">
        Two Hills Mennonite School
      </b-form-checkbox>
      <b-form-checkbox id="checkbox-6" name="checkbox-6" value="accepted">
        Vermilion Outreach School
      </b-form-checkbox>
      <b-form-checkbox id="checkbox-6" name="checkbox-6" value="accepted">
        Wetaskiwin Composite High School
      </b-form-checkbox>
    </b-modal>
  </div>
</template>

<script>
export default {
  name: "SchoolModal",
};
</script>

<style scoped></style>
