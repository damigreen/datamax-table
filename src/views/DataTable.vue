<template #data='pageNumber'>
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
    <!-- <TableBase  :columns="columns" :entries="filteredEntries" :pageNumber="paginateTable" :nextPage="nextPage" :prevPage="prevPage"  /> -->
    <TableBase
      :columns="columns"
      :entries="filteredEntries"
      :pageNumber="paginateTable"
      :numberOfPages="pageToDisplay"
    />
    <div class='show-info'>
      <div>Show {{ 4 }} to {{6}} of {{ this.currentEntry }}</div>
    </div>
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
      showEntries: [5, 8, 10, 12],
      currentEntry: 8,
      filteredEntries: [],
      pageNumber: 1,
      numberOfPages: 1
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
      console.log(res)
      this.filteredEntries = res;
    });
  },
  /* 
                    [1]                          [2]
          (-) [1] [2] [3] [4] [5] (+)    <-- [6] [7] [8] -->
  */
  computed: {
    filEnt() {
      this.numbering = 0;
      this.paginateEntries().then(res => {
        for (var i = 0; i < res.length; i++) {
          this.numbering += 1;
          // console.log(this.numbering);
        }
        this.filteredEntries = res;
      });
    },
    showInfo() {
      console.log('show info')
    },
    paginateTable () {
      return Math.ceil(this.entries.length / this.currentEntry);
    },
    pageToDisplay () {
      console.log(Math.ceil(this.entries.length / 8));
      return Math.ceil(this.entries.length / 8);
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
    },
    nextPage () {
      console.log('hhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhh');
      this.pageNumber++;
    },
    prevPage () {
      console.log(this.pageNumber--);
      
      this.pageNumber--;
    }
  }
}
</script>
