<template>
  <div class="container py-3">
    <h3>Formula 1 Top Drivers</h3>
    <div class="py-3 px-3">
      <div class="row">
        <div class="col">
          <label for="filters" class="form-label">Filter</label>
          <select class="form-select" id="filters" @change="selectFilter">
            <!-- TODO: render filter options -->
            <option v-for="item in filters" :key="item.id" :value="item.name">
              {{ item.name }}
            </option>
          </select>
        </div>
      </div>
      <div class="mb-3"></div>
      <div class="row">
        <div class="col">
          <div class="border border-1">
            <div class="row px-2 py-2">
              <div class="col">
                <div class="form-check">
                  <input
                      class="form-check-input"
                      type="checkbox"
                      id="selectAll"
                      :checked="allItemsSelected"
                      @change="(e) => selectAll(e)"
                  />
                  <label class="form-check-label" for="selectAll">
                    Select All
                  </label>
                </div>
              </div>
              <!-- TODO: render correct totals (i.e. 1 of 5 selected)-->
              <div class="col text-end">
                {{ selectedItems.length }} of {{ items.length }} selected
              </div>
            </div>
            <div class="row px-2 py-2">
              <div class="col">
                <!-- TODO: render collection of checkboxes -->
                <div v-for="item in activeItems" :key="item.id">
                  <div class="my-1">
                    <input
                        class="form-check-input"
                        type="checkbox"
                        :checked="false"
                        :id="item.id"
                        @change="(e) => selectItem(e, item.id)"
                    />
                    <label class="form-check-label mx-2" for="1">
                      {{ item.name }}
                    </label>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
const FILTER_OPTIONS = Object.freeze({
  ALL: {
    id: 1,
    name: "All",
  },
  SELECTED: {
    id: 2,
    name: "Selected",
  },
  UNSELECTED: {
    id: 3,
    name: "Unselected",
  },
});

export default {
  name: "ListBox",

  props: {
    items: {
      type: Array,
      default: () => [],
    },
  },

  data() {
    return {
      filters: Object.values(FILTER_OPTIONS),
      selectedFilter: null,
      selectedItems: [],
    };
  },

  computed: {
    activeItems() {
      // TODO: return the collection of items depending on their "status" (i.e. checked, unchecked or all)
      switch (this.selectedFilter) {
        case "Selected":
          const selected = this.items.filter((item) =>
              this.isItemChecked(item.id)
          );
          return selected;
        case "Unselected":
          const unselected = this.items.filter(
              (item) => !this.isItemChecked(item.id)
          );
          return unselected;
        default:
          return this.items;
      }
    },

    allItemsSelected() {
      // return boolean depending if all items are selected
      return this.selectedItems.length === this.items.length;
    },
  },

  methods: {
    isItemChecked(item) {
      // TODO: return boolean depending if the item is already in our selectedItems list
      if (!this.selectedItems.includes(item)) {
        return false;
      }
      return true;
    },

    selectFilter(e) {
      // TODO: set this.selectedFilter depending the e.target.value
      this.selectedFilter = e.target.value;
    },

    selectAll(e) {
      // TODO: select all or deselect all items depending on e.target.checked
      // TODO: emit selectedItems to parent component
      if (e.target.checked) {
        this.items.map((item) => {
          if (!this.isItemChecked(item.id)) {
            document.getElementById(item.id).checked = true;
            this.selectedItems.push(item.id);
          }
          return null;
        });
        this.$emit("selected-items", this.selectedItems);
      } else {
        this.items.map((item) => {
          const itemId = item.id;
          document.getElementById(itemId).checked = false;

          this.selectedItems = this.selectedItems.filter(
              (item) => item !== itemId
          );
          return null;
        });
        this.$emit("selected-items", this.selectedItems);
      }
    },

    selectItem(e, itemId) {
      // TODO: add or remove an item depending on e.target.checked
      // TODO: emit selectedItems to parent component
      if (e.target.checked) {
        if (!this.isItemChecked(itemId)) {
          this.selectedItems.push(itemId);
        }
        this.$emit("selected-items", this.selectedItems);
      } else {
        this.selectedItems = this.selectedItems.filter(
            (item) => item !== itemId
        );
        this.$emit("selected-items", this.selectedItems);
      }
    },
  },
};
</script>
