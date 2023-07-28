<template>
  <div class="container">
    <inputs-form @add-table="addTable" />
    <all-tables :tables="tables" @remove-table="removeTable" />
  </div>
</template>

<script setup>
import { reactive } from 'vue'
import InputsForm from './components/InputsForm.vue'
import AllTables from './components/AllTables.vue'

const tables = reactive([]);

const addTable = (tableData) => {
  tables.push(tableData)
}

const removeTable = (index) => {
  tables.splice(index, 1);
}
</script>

<style lang="scss">
/* Reset some default styles */
html, body, table {
  margin: 0;
  padding: 0;
  border: 0;
}

body {
  font-family: sans-serif;
  .container {
    max-width: 900px;
    margin: 0 auto;
    padding: 20px;
  }
}

</style>
