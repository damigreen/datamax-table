<template #data="{pageNumber}">
  <table class="table">
    <thead>
      <tr>
        <th v-for="(th) in tableHeader" :key="th">
          <div class="header-wrap">
            <span>{{ th.text }}</span>
          </div>
        </th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="td in TableBase" :key="td" class=head-wrap>
        <td>{{ td.name }}</td>
        <td>{{ td.isbn }}</td>
        <td >
          <div v-for="au in td.authors" :key="au">{{ au }}</div>
        </td>
        <td>{{ td.numberOfPages }}</td>
        <td>{{ td.country }}</td>
        <td>
          {{
            td.released.slice(0, 10)
          }}
        </td> 
      </tr>
    </tbody>
  </table>  

</template>

<script>

import './TableBase.css';

export default {
  name: 'TableBase',
  props: {
    columns: Array,
    entries: Array,
    pageNumber: Number,
    currentEntry: Number,
    numberOfPages: Number,
  },
  computed: {
    tableHeader() {
      return this.columns || [];
    },
    TableBase() {
      return this.entries || null;
    },
  },
}

</script>
