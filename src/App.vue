<template>
  <div id="app">
    <div>Example:</div>
    <br>
    <table>
      <thead>
        <tr>
          <th>id</th>
          <th class="s-20">Name</th>
          <th class="last-el"></th>
        </tr>
      </thead>

      <tbody>
        <tr
          v-for="(item, index) in list"
          :key="index"
        >
          <td>{{item.id}}</td>
          <td class="s-20">{{item.str}}</td>
          <td class="last-el"></td>
        </tr>
      </tbody>
    </table>
    
    <Pagination
      :entities="arr"
      :size="10"
      @change="onChange($event)"
    />
  </div>
</template>

<script>
import Pagination from '@/components/Pagination.vue';
import Mock from '@/mocks/mock';

export default {
  name: 'app',

  data() {
    return {
      list: [],
      arr: Mock,
    }
  },

  methods: {
    onChange(event) {
      this.list = event;
    },
  },

  components: { Pagination },
}
</script>

<style lang="scss">
#app {
  max-width: 70%;
  margin: 0 auto;
  padding-top: 50px; 
  font-family: sans-serif;

  .pagination {
    border-radius: 0;
    padding-top: 10px;

    &__btn {
      color: black;
      font-size: 13px;
      background-color: transparent;
      border: 1px solid #0f78f9;
      border-radius: 3px;

      &:hover {
        background-color: #0069d9;
        border-color: #0062cc;
        color: white;
      }
    }

    &-list__item {
      font-size: 12px;
      border: 1px solid rgb(205, 202, 202);

      &:hover {
        color: white;
        background-color: rgb(122, 169, 228);
        border-color: rgb(122, 169, 228);
      }

      &.active {
        color: white;
        border-color: rgb(15, 120, 249);
        background: rgb(15, 120, 249);
      }
    }

    .count-items,
    .count-pages {
      font-size: 12px;
      color: rgb(131, 131, 131);
    }
  }
}

table {
  border-collapse: collapse;
}

td, th {
  border: 1px solid rgb(173, 160, 160);
  padding: 5px;
}

.last-el {
  width: 100%;
}

.s-20 {
  min-width: 20em;
  max-width: 20em;
}
</style>