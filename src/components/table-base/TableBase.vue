<template #data="{pageNumber}">
  <table class="table">
    <thead>
      <tr>
        <th v-for="(th) in tableHeader" :key="th">
          <div class="header-wrap">
            <span>{{ th.text }}</span>
            <span style="display:flex, justify-content: center, align-items: center">
              <svg style="color: #DCDCDC" xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-caret-down-fill" viewBox="0 0 16 16">
                <path d="M7.247 11.14 2.451 5.658C1.885 5.013 2.345 4 3.204 4h9.592a1 1 0 0 1 .753 1.659l-4.796 5.48a1 1 0 0 1-1.506 0z"/>
            </svg>
            </span>
          </div>
        </th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="td in TableBase" :key="td" class=head-wrap>
      <!-- <tr v-for="td in paginatedData" :key="td" class=head-wrap> -->
        <td>{{ td.name }}</td>
        <td>{{ td.isbn }}</td>
        <td v-for="au in td.authors" :key="au">{{ au }}</td>
        <td>{{ td.numberOfPages }}</td>
        <td>{{ td.country }}</td>
        <!-- yyyy - mm - dd -->
        <td>{{ td.released }}</td> 
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
  },
  // methods: {
  //   prev () {
  //     console.log('--------------------------------------', this.pageNumber)
  //   },
  //   nextPage () {
  //     console.log('hhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhh');
  //     this.pageNumber = this.pageNumber + 1;
  //   }
  // },
}

</script>
