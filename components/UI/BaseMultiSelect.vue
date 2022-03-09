<template>
  <div v-click-outside="onClickOutside" class="theme-control--select">
    <base-input
      :readonly="!autocomplete"
      :value="searchValue"
      :label="label"
      :disabled="disabled"
      :error="!value.length ? error : EMPTY_STRING"
      append-icon="arr_expand"
      @focus="onFocus"
      @update:value="onChangeSearchValue"
    />

    <base-dropdown-list-multiple
      v-if="visible.dropdown && items.length > 0"
      :value="value"
      :items="items"
      class="dropdown-list"
      @update:value="onChangeValue"
      @add-value="onAddValue"
      @remove-value="onRemoveValue"
    />

    <div v-if="value.length" class="grid--flex grid--flex-wrap m--t--04 m--b-04">
      <div class="m--r-02 m--b-02 theme-tag-button theme-tag-button--main theme-tag-button--small checked"
           v-for="(item, index) in value"
           :key="item + index"
           @click="removeItem(index)"
      >
        <div class="theme-tag-button--title">
          <span class="theme-tag-button--label">{{item.name}}</span>
        </div>
        <div class="w--4 m--r-02 h--full grid--flex-all-center">
          X
        </div>
      </div>
<!--      <base-tag-button-->
<!--        v-for="(item, index) in value"-->
<!--        :key="item + index"-->
<!--        size="small"-->
<!--        color="main"-->
<!--        :label="item"-->
<!--        :value="true"-->
<!--        class="m&#45;&#45;r-02 m&#45;&#45;b-02"-->
<!--        @change="removeItem(index)"-->
<!--      />-->
    </div>
  </div>
</template>

<script>
  import BaseInput from './BaseInput'
  import BaseDropdownListMultiple from './BaseDropdownListMultiple'
  import { EMPTY_STRING, EMPTY_ARRAY } from '../../const/default'
  import clickOutside from '../../utils/directives/clickOutside'

  export default {
    name: 'BaseMultiSelect',
    components: {
      BaseInput,
      BaseDropdownListMultiple,
    },
    directives: { clickOutside },
    props: {
      label: {
        type: String,
        default: EMPTY_STRING,
      },
      value: {
        type: Array,
        default: () => EMPTY_ARRAY,
      },
      items: {
        type: Array,
        default: () => EMPTY_ARRAY,
      },
      disabled: {
        type: Boolean,
        default: false,
      },
      error: {
        type: String,
        default: EMPTY_STRING,
      },
      searchValue: {
        type: String,
        default: EMPTY_STRING,
      },
      autocomplete: {
        type: Boolean,
        default: false,
      },
    },
    data() {
      return {
        EMPTY_STRING,
        focused: false,
        visible: {
          dropdown: false,
        },
      }
    },
    methods: {
      removeItem(index) {
        this.onChangeValue(this.value.filter((item, i) => i !== index))
        this.onRemoveValue(this.value[index])
      },
      onChangeValue(value) {
        this.$emit('update:value', value)
      },
      onClickOutside() {
        if (this.focused) {
          this.onBlur()
        }

        if (this.visible.dropdown) {
          this.hideDropdownList()
        }
      },
      hideDropdownList() {
        this.visible.dropdown = false
      },
      showDropdownList() {
        this.visible.dropdown = true
      },
      onFocus() {
        this.focused = true
        this.showDropdownList()
        this.$emit('focus')
      },
      onBlur() {
        this.focused = false
        this.$emit('blur')
      },
      onChangeSearchValue(value) {
        this.$emit('update:search-value', value)
        this.showDropdownList()
      },
      onAddValue(value) {
        this.$emit('add-value', value)
      },
      onRemoveValue(value) {
        this.$emit('remove-value', value)
      },
    },
  }
</script>

<style lang="scss" scoped>
  .m--t--04 {
    margin-top: -16px;
  }
</style>
