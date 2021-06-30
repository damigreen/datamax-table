<template>
  <TableData />
</template>

<script>
import TableData from '../components/tableData/TableData.vue';

export default {
  name: 'DataTable',
  data() {
    return {
      columns: [
        { name: 'name', text: 'Name'},
        { name: 'isbn', text: 'ISBN'},
        { name: 'authors', text: 'Authors'},
        { name: 'number_of_pages', text: 'Nummber of Pages'},
        { name: 'country', text: 'Country'},
        { name: 'released', text: 'Released'},
      ]
    }
  }
}
</script>