<template>
  <footer
    v-if="isFooterVisible"
    class="bg-white dark:bg-slate-800 h-[60px] border-t border-solid border-slate-75 dark:border-slate-700/50 flex items-center justify-between py-0 px-4"
  >
    <div class="left-aligned-wrap">
      <div class="text-xs text-slate-600 dark:text-slate-200">
        <strong>{{ firstIndex }}</strong>
        - <strong>{{ lastIndex }}</strong> of
        <strong>{{ totalCount }}</strong> items
      </div>
    </div>
    <div class="right-aligned-wrap">
      <div
        v-if="totalCount"
        class="primary button-group pagination-button-group"
      >
        <woot-button
          size="small"
          variant="smooth"
          color-scheme="secondary"
          class-names="goto-first"
          :is-disabled="hasFirstPage"
          @click="onFirstPage"
        >
          <fluent-icon icon="chevron-left" size="18" />
          <fluent-icon
            icon="chevron-left"
            size="18"
            :class="pageFooterIconClass"
          />
        </woot-button>
        <woot-button
          size="small"
          variant="smooth"
          color-scheme="secondary"
          :is-disabled="hasPrevPage"
          @click="onPrevPage"
        >
          <fluent-icon icon="chevron-left" size="18" />
        </woot-button>
        <woot-button
          size="small"
          variant="smooth"
          color-scheme="secondary"
          @click.prevent
        >
          {{ currentPage }}
        </woot-button>
        <woot-button
          size="small"
          variant="smooth"
          color-scheme="secondary"
          :is-disabled="hasNextPage"
          @click="onNextPage"
        >
          <fluent-icon icon="chevron-right" size="18" />
        </woot-button>
        <woot-button
          size="small"
          variant="smooth"
          color-scheme="secondary"
          class-names="goto-last"
          :is-disabled="hasLastPage"
          @click="onLastPage"
        >
          <fluent-icon icon="chevron-right" size="18" />
          <fluent-icon
            icon="chevron-right"
            size="18"
            :class="pageFooterIconClass"
          />
        </woot-button>
      </div>
    </div>
  </footer>
</template>

<script>
import rtlMixin from 'shared/mixins/rtlMixin';

export default {
  components: {},
  mixins: [rtlMixin],
  props: {
    currentPage: {
      type: Number,
      default: 1,
    },
    pageSize: {
      type: Number,
      default: 25,
    },
    totalCount: {
      type: Number,
      default: 0,
    },
  },
  computed: {
    pageFooterIconClass() {
      return this.isRTLView ? '-mr-3' : '-ml-3';
    },
    isFooterVisible() {
      return this.totalCount && !(this.firstIndex > this.totalCount);
    },
    firstIndex() {
      return this.pageSize * (this.currentPage - 1) + 1;
    },
    lastIndex() {
      return Math.min(this.totalCount, this.pageSize * this.currentPage);
    },
    searchButtonClass() {
      return this.searchQuery !== '' ? 'show' : '';
    },
    hasLastPage() {
      return !!Math.ceil(this.totalCount / this.pageSize);
    },
    hasFirstPage() {
      return this.currentPage === 1;
    },
    hasNextPage() {
      return this.currentPage === Math.ceil(this.totalCount / this.pageSize);
    },
    hasPrevPage() {
      return this.currentPage === 1;
    },
  },
  methods: {
    onNextPage() {
      if (this.hasNextPage) {
        return;
      }
      const newPage = this.currentPage + 1;
      this.onPageChange(newPage);
    },
    onPrevPage() {
      if (this.hasPrevPage) {
        return;
      }
      const newPage = this.currentPage - 1;
      this.onPageChange(newPage);
    },
    onFirstPage() {
      if (this.hasFirstPage) {
        return;
      }
      const newPage = 1;
      this.onPageChange(newPage);
    },
    onLastPage() {
      if (this.hasLastPage) {
        return;
      }
      const newPage = Math.ceil(this.totalCount / this.pageSize);
      this.onPageChange(newPage);
    },
    onPageChange(page) {
      this.$emit('page-change', page);
    },
  },
};
</script>

<style lang="scss" scoped>
.goto-first,
.goto-last {
  i:last-child {
    @apply -ml-1;
  }
}
</style>
