<template>
  <div class="custom-select-wrapper">
    <div
      class="custom-select-header"
      @click="toggleDropdown"
      :class="{ active: dropdownVisible }"
    >
      {{ selectedOption }}
    </div>
    <ul v-show="dropdownVisible" class="custom-select-dropdown">
      <li
        v-for="option in options"
        :key="option.value"
        @click="selectOption(option)"
      >
        {{ option.label }}
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  props: {
    options: {
      type: Array,
      required: true,
    },
    value: {
      type: String | Number,
      required: true,
    },
  },
  data() {
    return {
      dropdownVisible: false,
    };
  },
  computed: {
    selectedOption() {
      const option = this.options.find((option) => option.value === this.value);
      return option ? option.label : "";
    },
  },
  methods: {
    toggleDropdown() {
      this.dropdownVisible = !this.dropdownVisible;
    },
    selectOption(option) {
      this.$emit("input", option.value);
      this.toggleDropdown();
    },
    closeDropdown(event) {
      if (!this.$el.contains(event.target)) {
        this.dropdownVisible = false;
      }
    },
  },
  mounted() {
    document.addEventListener("click", this.closeDropdown);
  },
  beforeDestroy() {
    document.removeEventListener("click", this.closeDropdown);
  },
};
</script>

<style lang="scss">
.custom-select-wrapper {
  position: relative;
  display: inline-block;
}

.custom-select-header {
  display: flex;
  align-items: center;
  width: 100%;
  box-sizing: border-box;
  padding: 0 12px 0 12px;
  height: 45px;
  border-radius: 5px;
  margin: 5px 0;
  border: 1px solid #121116;
  transition: all 0.1s linear;
  background-color: white;
  cursor: pointer;
  &.active {
    border: 1px solid #cececf;
  }
}

.custom-select-dropdown {
  position: absolute;
  top: 100%;
  left: 0;
  z-index: 1;
  padding: 0;
  margin: 0;
  width: 100%;
  border-radius: 5px;
  background-color: #fff;
  border: 1px solid #cececf;
  box-shadow: 0 2px 2px rgba(0, 0, 0, 0.1);
  list-style: none;
  cursor: pointer;
}

.custom-select-dropdown li {
  padding: 8px 16px;
}

.custom-select-dropdown li:hover {
  background-color: #f1f1f1;
}
</style>
