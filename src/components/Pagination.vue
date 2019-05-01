<template>
  <div class="pagination">
    <div class="pagination__paginator">
      <div>
        <button class="pagination__btn pagination__btn_first" @click="pageFirst()" type="button">
          <slot name="page-first"><span>First</span></slot>
        </button>
        <button class="pagination__btn pagination__btn_prev" @click="pagePrev()" type="button">
          <slot name="page-prev"><span>Prev</span></slot>
        </button>
      </div>
      <div class="pagination-list">
        <div
          class="pagination-list__item"
          v-for="page in paginationPageArray"
          :key="page"
          :class="{'active': currentPage === page}"
          @click="goToPage(page)"
        >
          <span>{{page}}</span>
        </div>
      </div>
      <div>
        <button class="pagination__btn pagination__btn_next" @click="pageNext()" type="button">
          <slot name="page-next"><span>Next</span></slot>
        </button>
        <button class="pagination__btn pagination__btn_last" @click="pageLast()" type="button">
          <slot name="page-last"><span>Last</span></slot>
        </button>
      </div>
      <div class="count-pages" v-if="showPageOfPages">
        {{currentPage}} - {{pageCount}} pages
      </div>
    </div>
    <div class="count-items" v-if='showItemOfItems'>
      {{entities.length === 0 ? 0 : from + 1}} - {{to}} of
      {{entities.length}} items
    </div>
  </div>
</template>

<script>
export default {
  name: 'Pagination',

  props: {
    size: { type: Number, default: 5 },
    showBy: { type: Number, default: 5 },
    entities: { type: Array, required: true },
    showPageOfPages: { type: Boolean, default: true },
    showItemOfItems: { type: Boolean, default: true },
  },

  data() {
    return {
      to: 0,
      from: 0,
      currentPage: 1,
    }
  },

  watch: {
    entities: {
      immediate: true,
      handler(value) {
        this.$emit('change', this.slicedEntities);
      }
    },
  },

  computed: {
    pageCount() {
      return Math.ceil(this.entities.length / this.size);
    },

    paginationPageArray() {
      const pages = this.pageCount + 1;
      const currentPage = this.currentPage;
      const showBy = this.showBy;
      const moveAfter = Math.ceil(showBy / 2);
      let arr = [];
      for (let i = 1; i < pages; i++) {
        arr.push(i);
      }
      if (currentPage < moveAfter || arr.length < showBy) {
        return arr.slice(0, showBy);
      }
      const endPoint = arr.length - moveAfter;
      if (currentPage > endPoint) {
        return arr.slice(arr.length - showBy, arr.length);
      }
      const from = this.currentPage - moveAfter;
      const to = from + showBy;
      return arr.slice(from, to);
    },

    slicedEntities() {
      const entities = this.entities;
      this.from = (this.currentPage - 1) * this.size;
      this.to = this.from + this.size;
      
      if (this.to > entities.length) {
        this.to = entities.length;
      }

      let sliced = entities.slice(this.from, this.to);

      if (sliced.length === 0 && entities.length !== 0) {
        sliced = entities.slice(this.from - this.size, this.to);
        this.pagePrev();
      }

      return sliced;
    },
  },

  methods: {
    goToPage(page) {
      this.currentPage = page;
      this.$emit('change', this.slicedEntities);
    },

    pagePrev() {
      if (this.currentPage === 1 || this.pageCount === 0) return;
      this.currentPage--;
      this.$emit('change', this.slicedEntities);
    },

    pageNext() {
      if (this.currentPage >= this.pageCount || this.pageCount === 0) return;
      this.currentPage++;
      this.$emit('change', this.slicedEntities);
    },

    pageFirst() {
      if (this.currentPage === 1 || this.pageCount === 0) return;
      this.currentPage = 1;
      this.$emit('change', this.slicedEntities);
    },

    pageLast() {
      if (this.currentPage === this.pageCount || this.pageCount === 0) return;
      this.currentPage = this.pageCount;
      this.$emit('change', this.slicedEntities);
    },
  },
}
</script>

<style lang="scss" scoped>
* {
  box-sizing: border-box;
}

.pagination {
  display: flex;
  align-items: center;
  justify-content: space-between;

  &__paginator {
    display: flex;
    align-items: center;
  }

  .count-pages {
    display: flex;
    align-items: center;
    margin-left: 15px;
  }

  .count-items {
    display: flex;
    align-items: center;
  }
}

.pagination__btn {
  min-width: 60px;
  min-height: 30px;
  margin-right: 5px;
  cursor: pointer;

  &_next {
    margin-left: 5px;
  }

  &_last {
    margin-right: 0;
  }
}

.pagination-list {
  display: flex;
  align-items: center;

  &__item {
    display: flex;
    align-items: center;
    justify-content: center;
    width: 30px;
    height: 30px;
    border: 1px solid black;
    cursor: pointer;
    user-select: none;

    &.active {
      background-color: silver;
    }
  }
}
</style>
