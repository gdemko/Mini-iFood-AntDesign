<template>
  <a-form-item
    :id="`input-group-${name}`"
    :label="label"
  >
    <a-input :state="stateError" :name="name" :id="`input-${name}`" @input="handleInput" :required="required" :value="initialValue" :placeholder="placeholder"></a-input>
    <span v-if="stateError">
      {{ messageError}}
    </span>
  </a-form-item>
</template>

<script>
export default {
  props:{
    value: {
      type: String,
      default: ""
    },
    label:{
      type: String,
    },
    description: {
      type: String
    },
    type: {
      type: String,
      default: 'text'
    },
    placeholder: {
      type: String,
    },
    name: {
      type: String,
      required: true
    },
    required: {
      type: String,
    },
    stateError: {
      type: Boolean,
      default: null
    },
    messageError: {
      type: String
    },
  },
  data() {
    return {
      size: "",
      initialValue: this.value
    }
  },
  methods: {
    handleInput(e)
    {
      this.$emit("input", e.target.value);
    },
  },
}
</script>
