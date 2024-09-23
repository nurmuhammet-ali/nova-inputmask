<template>
  <DefaultField :field="currentField" :errors="errors" :show-help-text="showHelpText">
    <template #field>
      <input
        :id="currentField.attribute"
        type="text"
        class="w-full form-control form-input form-control-bordered"
        :class="errorClasses"
        :placeholder="currentField.name"
        v-model="value"
      />
    </template>
  </DefaultField>
</template>

<script>
import { DependentFormField, HandlesValidationErrors } from 'laravel-nova'
import Inputmask from "inputmask"

export default {
  mixins: [DependentFormField, HandlesValidationErrors],

  props: ['resourceName', 'resourceId', 'field'],

  methods: {
    /*
     * Set the initial, internal value for the field.
     */
    setInitialValue() {
      this.value = this.field.value || ''
    },

    /**
     * Fill the given FormData object with the field's internal value.
     */
    fill(formData) {
      this.value = (
          this.field.mask && this.field.storeRawValue
      ) ? document.getElementById(this.field.attribute).inputmask.unmaskedvalue() : this.value

      formData.append(this.field.attribute, this.value || '')
    },

    maskField() {
      const field = document.getElementById(this.field.attribute)
      
      new Inputmask(this.field.mask).mask(field);
    }
  },

  mounted() {
    if (this.field.phone_number && this.field.country == 'TM') {
      this.field.mask = '+(\\9\\93)-69-99-99-99';
    }

    if (this.field.mask) {
      this.maskField()   
    }
  }
}
</script>
