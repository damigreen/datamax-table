<template>
  <table class="table">
    <thead>
      <tr>
        <th>
          <div v-for="th in tableHeader" :key="th" class=head-wrap>
            <span>{{ th }}</span>
            <span>
              <svg></svg>


            </span>
          </div>
        </th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="td in tableHeader" :key="td" class=head-wrap>
        <td>{{ td.name }}</td>
        <td>{{ td.isbn }}</td>
        <td>{{ td.authors }}</td>
        <td>{{ td.number_of_pages }}</td>
        <td>{{ td.country }}</td>
        <!-- yyyy - mm - dd -->
        <td>{{ td.released }}</td> 
      </tr>
    </tbody>
  </table>  
</template>

<script>

import './TableData.css';

export default {
  name: 'TableData',
  props: {
    columns: Array,
    entries: Array
  },
  computed: {
    tableHeader() {
      return this.columns || []
    },
    tableData() {
      return this.entries || null
    }
  }
}

</script>
