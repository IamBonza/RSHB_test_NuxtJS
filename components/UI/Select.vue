<template>
  <b-select placeholder="Select a name">
    <option
      v-for="option in data"
      :value="option.id"
      :key="option.id">
      {{ option.user.first_name }}
    </option>
  </b-select>
</template>
