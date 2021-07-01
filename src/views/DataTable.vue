<template #data='pageNumber'>
  <div class="table-wrap">
    <div class='filter-wrapper'>
      <div class="flex-row">Show
        <span class="mg-r">
          <!-- <select v-model="currentEntry" @change="paginateEntries" class="select"> -->

          <select v-model="currentEntry" @change="filEnt" class="select">
            <option v-for="se in showEntries" :key="se" :value="se" >{{ se }}</option>
          </select>
        </span> entries
      </div>

      <div class='search-wrap'>
        <input type="text" v-model="search" @keyup="filteredList"  placeholder="Search book.."/>
        <!-- <input type="text" v-model="search" v-on="filteredList" placeholder="Search book.."/> -->
        <label>Search book:</label>
      </div>

    </div>

    <!-- <TableBase :columns="columns" :entries="entries"  /> -->
    <!-- <TableBase  :columns="columns" :entries="filteredEntries" :pageNumber="paginateTable" :nextPage="nextPage" :prevPage="prevPage"  /> -->
    <TableBase
      :columns="columns"
      :entries="filteredEntries"
      :pageNumber="paginateTable"
      :numberOfPages="pageToDisplay"
      @prevPage="prevPage"
    />
    <div class='show-info'>
      <div>Show {{ 4 }} to {{6}} of {{ this.currentEntry }}</div>
    </div>

      <div class='pagination-wrap'>
      <!-- <span> -->
      <!-- 
            (-) [1] [2] [3] [4] [5] (+)    - [6] [7] [8] +
      -->

    <button :disabled="pageNumber === 1"  @click='prevPage' :key="1" class='paginate-button'>PREV</button>
    <span class="paginate-number" v-for="(item, i) in 2" :key="item">
        <button >{{ i + 1 }} [{{pageNumber}}]</button>
    </span>
    <button :disabled="pageNumber >= 2" @click='nextPage' class='paginate-button' style="margin-left: 4px">NEXT</button>

    <slot name='data' :pageNumber='pageNumber' :numberOfPages='numberOfPages' />
  </div>
  <span :key='this.pageNumber' style='background: red; padding: 5px'>{{this.pageNumber}}</span>
  <span :key='this.numberOfPages' style='background: green; padding: 5px'>{{this.numberOfPages}}</span>

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
      numberOfPages: 1,
      paginatedDataArray: [],
      search: '',
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
  /* 
                    [1]                          [2]
          (-) [1] [2] [3] [4] [5] (+)    <-- [6] [7] [8] -->
  */
  computed: {
    filEnt() {
      this.numbering = 0;
      this.paginateEntries().then(res => {
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
      return Math.ceil(this.entries.length / 8);
    },
    paginatedData () {
      const num = Math.ceil(this.entries.length / 8)
      console.log('num====================',num);
      const start = (this.pageNumber * 8) - (8 - 1);
      const end = start + (8 - 1);
      console.log('---------------------');
      console.log(this.pageNumber)
      console.log('start============',start-1);
      console.log('end============',end);
      this.filteredEntries = this.entries.slice(start - 1, end)  
    },
    filteredList () {
      const matchName = this.entries.filter(entr => {
        
        console.log(entr.name.toLowerCase())
        console.log(this.search.toLowerCase())
        const ifMatched = entr.name.toLowerCase().includes(this.search.toLowerCase()) 
        console.log(ifMatched);
        return ifMatched ? entr.name : null;


      });
      this.filteredEntries = matchName;

      const matchAuthor = this.entries.filter(entr => {
        
        console.log(entr.name.toLowerCase());
        console.log(this.search.toLowerCase());
        const ifMatched = entr.name.toLowerCase().includes(this.search.toLowerCase()) 
        console.log(ifMatched);
        return ifMatched ? entr.name : null;


      });
      this.filteredEntries = matchAuthor;
    },
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
      this.pageNumber = this.pageNumber + 1;
      this.paginatedData
    },
    prevPage () {
      this.pageNumber = this.pageNumber - 1;
      this.paginatedData;
    },
    // paginatedData () {
    //   const start = (this.pageNumber * 8) - (8 - 1);
    //   const end = start + (8 - 1);
    //   console.log(this.pageNumber)
    //   console.log('---------------------')
    //   console.log(start);
    //   console.log(end)
    //   this.pageNumber = this.pageNumber + 1;
    //   this.entries = this.entries.slice(start, end)
    // },
  }
}
</script>
