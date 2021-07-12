<template>
  <div>
    <!-- <b-button v-b-modal.modal-scrollable><slot></slot></b-button> -->
    <slot />
    <b-modal id="group-modal" scrollable title="Select Groups">
      <b-form-checkbox id="checkbox-1" name="checkbox-1" value="accepted">
        Federal
      </b-form-checkbox>
      <b-form-checkbox id="checkbox-2" name="checkbox-2" value="accepted">
        First Nations
      </b-form-checkbox>
      <b-form-checkbox id="checkbox-3" name="checkbox-3" value="accepted">
        Francophone
      </b-form-checkbox>
      <b-form-checkbox id="checkbox-4" name="checkbox-4" value="accepted">
        Private
      </b-form-checkbox>
      <b-form-checkbox id="checkbox-5" name="checkbox-5" value="accepted">
        Provincial
      </b-form-checkbox>
      <b-form-checkbox id="checkbox-6" name="checkbox-6" value="accepted">
        Public
      </b-form-checkbox>
      <b-form-checkbox id="checkbox-6" name="checkbox-6" value="accepted">
        Separate
      </b-form-checkbox>
    </b-modal>
  </div>
</template>

<script>
export default {
  name: "GroupModal",
};
</script>

<style scoped></style>
