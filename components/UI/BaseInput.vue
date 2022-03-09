<template>
  <div>
    <div class="theme-control--input" :class="classes.input">
      <label class="label" :for="id">{{ label }}</label>
      <div class="grid--flex grid--align-center h--full">
        <input
          :id="id"
          :value="value"
          :disabled="disabled"
          :readonly="readonly"
          type="text"
          @input="onUpdateValue($event.target.value)"
          @change="onChangeValue($event.target.value)"
          @focus="onFocus"
          @blur="onBlur"
        />
        <div v-if="visible.clearButton" class="icon" @click="onClearValue">
          Delete
        </div>
        <label v-if="appendIcon" :for="id" class="icon append">
        </label>
      </div>
    </div>
    <div v-if="!noPaddingBottom" class="h--6">
      <span v-if="visible.errorMessage" class="theme-control--error">{{ error }}</span>
    </div>
  </div>
</template>

<script>
  import { EMPTY_STRING } from '../../const/default'
  // import BaseIcon from '../icons/BaseIcon.vue'

  export default {
    name: 'BaseInput',
    components: {
      // BaseIcon,
    },
    props: {
      label: {
        type: String,
        default: EMPTY_STRING,
      },
      value: {
        type: String,
        default: EMPTY_STRING,
      },
      disabled: {
        type: Boolean,
        default: false,
      },
      clearable: {
        type: Boolean,
        default: false,
      },
      error: {
        type: String,
        default: EMPTY_STRING,
      },
      appendIcon: {
        type: String,
        default: null,
      },
      readonly: {
        type: Boolean,
        default: false,
      },
      noPaddingBottom: {
        type: Boolean,
        default: false,
      },
    },
    data() {
      return {
        EMPTY_STRING,
        id: null,
        focused: false,
      }
    },
    computed: {
      classes() {
        return {
          input: {
            focused: this.focused,
            disabled: this.disabled,
            invalid: this.error,
            active: (!this.readonly && this.focused) || this.value.length,
          },
        }
      },
      visible() {
        return {
          clearButton: this.clearable && !this.disabled && Boolean(this.value.length),
          errorMessage: this.error && !this.focused,
        }
      },
    },
    mounted() {
      // объявляется в mounted, т.к. объявленный в data() - криво работает
      this.id = `BaseInput_${this._uid}`
    },
    methods: {
      onUpdateValue(value) {
        this.$emit('update:value', value)
        this.$emit('input', value)
      },
      onChangeValue(value) {
        this.$emit('change:value', value)
      },
      onClearValue() {
        this.onUpdateValue(EMPTY_STRING)
      },
      onFocus() {
        this.focused = true
        this.$emit('focus')
      },
      onBlur() {
        this.focused = false
        this.$emit('blur')
      },
    },
  }
</script>
