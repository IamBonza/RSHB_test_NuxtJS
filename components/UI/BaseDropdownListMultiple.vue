<template>
  <div class="theme-control--dropdown-list multiple-list">
    <div
      v-for="(item, index) in items"
      :key="`list-item-${index}`"
      class="list-item"
      :class="{ active: isSelected(item) }"
      @click="onItemClick(item)"
    >
      <span class="item-title">{{ item.name }}</span>

      <div class="append-icon">
      </div>
    </div>
  </div>
</template>

<script>
  import { EMPTY_ARRAY } from '../../const/default'
  // import BaseIcon from '../icons/BaseIcon.vue'

  export default {
    name: 'BaseDropdownListMultiple',
    components: {
      // BaseIcon,
    },
    props: {
      value: {
        type: Array,
        default: () => EMPTY_ARRAY,
      },
      items: {
        type: Array,
        default: () => EMPTY_ARRAY,
      },
    },
    data() {
      return {
        localValue: [...this.value],
      }
    },
    watch: {
      value(newValue) {
        this.localValue = [...newValue]
      },
    },
    methods: {
      isSelected(item) {
        return this.value.includes(item)
      },
      onItemClick(item) {
        const index = this.localValue.findIndex((value) => value === item)

        if (index === -1) {
          this.localValue.push(item)
          this.onAddValue(item)
        } else {
          this.localValue.splice(index, 1)
          this.onRemoveValue(item)
        }

        this.$emit('update:value', this.localValue)
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
