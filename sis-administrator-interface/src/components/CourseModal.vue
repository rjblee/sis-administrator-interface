<template>
  <div>
    <!-- <b-button v-b-modal.modal-scrollable><slot></slot></b-button> -->
    <slot />
    <b-modal id="modal-scrollable" scrollable title="Select Courses">
      <b-form-checkbox id="checkbox-1" name="checkbox-1" value="accepted">
        BEAR SAFETY AWARENESS - SK
      </b-form-checkbox>
      <b-form-checkbox id="checkbox-2" name="checkbox-2" value="accepted">
        ASBESTOS AWARENESS- BC
      </b-form-checkbox>
      <b-form-checkbox id="checkbox-3" name="checkbox-3" value="accepted">
        ALBERTA DRIVER HOURS OF SERVICE LIMITS
      </b-form-checkbox>
      <b-form-checkbox id="checkbox-4" name="checkbox-4" value="accepted">
        BULLYING PREVENTION
      </b-form-checkbox>
      <b-form-checkbox id="checkbox-5" name="checkbox-5" value="accepted">
        COLD STRESS AWARENESS
      </b-form-checkbox>
      <b-form-checkbox id="checkbox-6" name="checkbox-6" value="accepted">
        EMERGENCY RESPONSE PLANNING (ERP)
      </b-form-checkbox>
    </b-modal>
  </div>
</template>

<script>
export default {
  name: "CourseModal",
};
</script>

<style scoped></style>
