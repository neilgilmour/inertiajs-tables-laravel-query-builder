<script>
export default {
  props: {
    meta: {
      type: Object,
      default: () => {
        return {};
      },
      required: false,
    },

    columns: {
      type: Object,
      default: () => {
        return {};
      },
      required: false,
    },

    filters: {
      type: Object,
      default: () => {
        return {};
      },
      required: false,
    },

    search: {
      type: Object,
      default: () => {
        return {};
      },
      required: false,
    },

    onUpdate: {
      type: Function,
      required: false,
    },
  },

  computed: {
    hasFilters() {
      return Object.keys(this.filters || {}).length > 0;
    },

    hasColumns() {
      return Object.keys(this.columns || {}).length > 0;
    },

    hasSearchRows() {
      return Object.keys(this.search || {}).length > 0;
    },
  },

  data() {
    return {
      newSearch: [],
      queryBuilderData: {
        columns: this.columns,
        filters: this.filters,
        search: this.search,
      },
    };
  },

  methods: {
    disableSearch(key) {
      this.newSearch = this.newSearch.filter((search) => search != key);

      this.queryBuilderData.search[key].enabled = false;
      this.queryBuilderData.search[key].value = null;
    },

    enableSearch(key) {
      this.queryBuilderData.search[key].enabled = true;
      this.newSearch.push(key);

      this.$nextTick(() => {
        this.$refs["rows"].focus(key);
      });
    },

    //

    changeSearchValue(key, value) {
      this.queryBuilderData.search[key].value = value;
    },

    changeGlobalSearchValue(value) {
      this.changeSearchValue("global", value);
    },

    changeFilterValue(key, value) {
      this.queryBuilderData.filters[key].value = value;
    },

    changeColumnStatus(column, status) {
      this.queryBuilderData.columns[column].enabled = status;
    },
  },

  watch: {
    queryBuilderData: {
      deep: true,
      handler() {
        if (this.onUpdate) {
          this.onUpdate(this.queryBuilderData);
        }
      },
    },
  },

  mounted() {
    Object.keys(this.queryBuilderData.columns).forEach((key) => {
      this.changeColumnStatus(key, this.queryBuilderData.columns[key].show);
    })
  }
};
</script>