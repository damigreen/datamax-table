<template>
  <div class="table-wrap">
    <div class="flex-row">Show
      <span class="mg-r">
        <!-- <select v-model="currentEntry" @change="paginateEntries" class="select"> -->
        <select v-model="currentEntry" @change="filEnt" class="select">
          <option v-for="se in showEntries" :key="se" :value="se" >{{ se }}</option>
        </select>
      </span> entries
    </div>

    <!-- <TableBase :columns="columns" :entries="entries"  /> -->
    <TableBase :columns="columns" :entries="filteredEntries"  />
  </div>

</template>
<script>

import './DataTable.css';
import TableBase from '../components/table-base/TableBase.vue';

export default {
  name: 'DataTable',
  data() {
    return {
      columns: [
        { name: 'name', text: 'Name'},
        { name: 'isbn', text: 'ISBN'},
        { name: 'authors', text: 'Authors'},
        { name: 'number_of_pages', text: 'Number of Pages'},
        { name: 'country', text: 'Country'},
        { name: 'released', text: 'Released'},
      ],

      entries: [],
      showEntries: [5, 10, 12],
      currentEntry: 5,
      filteredEntries: []
    }
  },
  components: {
    TableBase
  },
  created() {
    this.getAllBooks().then(books => {
      this.entries = books;
    });

    this.paginateEntries().then(res => {
      this.filteredEntries = res;
    });
  },
  computed: {
    filEnt() {
      this.paginateEntries().then(res => {
        this.filteredEntries = res;
      })
    }
  },
  methods: {
    async getAllBooks() {
      const response = await fetch("https://www.anapioficeandfire.com/api/books");
      return response.json();
    },
    async paginateEntries() {
      const response = await fetch(`https://www.anapioficeandfire.com/api/books?page=1&pageSize=${this.currentEntry}`);
      return response.json();
    }
  }
}
</script>
