<template>
  <div class="pagination">
    <button
      v-if="showPrevious"
      class="item-prev"
      @click="changePage(offset - 1, 'x', 'y')"
    >
      &laquo;
    </button>
    <button v-if="showPrevious" class="item" @click="changePage(0)">1</button>
    <button v-if="showPrevious" class="item">...</button>

    <button
      v-for="page in pages"
      :key="page"
      class="item"
      :class="{ current: page + 1 === current }"
      @click="changePage(page, page, pages)"
    >
      {{ page + 1 }}
    </button>
    <button v-if="showNext" class="item">...</button>
    <button
      v-if="showNext"
      class="item"
      @click="changePage(total - 1, 'x', 'y')"
    >
      {{ this.total }}
    </button>
    <button
      v-if="showNext"
      class="item-next"
      @click="changePage(offset + 1, 'x', 'y')"
    >
      &raquo;
    </button>
  </div>
</template>

<script>
export default {
  name: "ContentPagination",
  props: {
    offset: {
      type: [String, Number],
      default: 0,
    },
    total: {
      type: [String, Number],
      required: true,
    },
    limit: {
      type: [String, Number],
      default: 10,
    },
  },
  watch: {},

  computed: {
    showPrevious() {
      return this.offset > 0;
    },
    showNext() {
      return this.offset < this.total - 1;
    },
    current() {
      return this.offset ? this.offset + 1 : 1;
    },
    pages() {
      if (this.total <= 1) return [1];
      const rowL = 6;
      const jam = Array.from(Array(this.total).keys(), (i) => i);
      console.log(jam);
      if (this.offset < rowL / 2) {
        return jam.slice(0, rowL);
      }
      if (this.offset >= this.total - rowL / 2) {
        return jam.slice(this.total - 1 - rowL, this.total);
      }

      return jam.slice(this.offset - rowL / 2, this.offset + (rowL + 2) / 2);
    },
  },
  methods: {
    changePage(offset) {
      this.$emit("change-page", offset);
    },
  },
};
</script>

<style lang="scss" scoped>
$light-grey: #cccccc;
$dark-grey: #333333;
$orange: #af57d8;
$dark-orange: #580b7c;
.pagination {
  display: flex;
  justify-content: center;
  flex-wrap: wrap;

  .item {
    padding: 0.5rem 0.75rem;
    width: 4em;
    border: 1px solid $light-grey;
    cursor: pointer;
    background-color: white;
    &:first-child {
      border-top-left-radius: 3px;
      border-bottom-left-radius: 3px;
    }
    &:last-child {
      border-top-right-radius: 3px;
      border-bottom-right-radius: 3px;
    }
    &:hover {
      background-color: $light-grey;
      border-color: lighten($dark-grey, 50%);
      z-index: 3;
    }
    &.current {
      cursor: default;
      color: white;
      background-color: $orange;
      border-color: $dark-orange;
      z-index: 2;
    }
    + .item {
      margin-left: -1px;
      margin-right: 0;
    }
  }
}
</style>
