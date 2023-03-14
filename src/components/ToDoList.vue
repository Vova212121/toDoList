<template>
  <div class="to-do">
    <h1 class="to-do__title">To do List</h1>
    <div class="to-do__header">
      <div class="to-do__header__section">
        <span class="to-do__header__section__title">Status</span>
        <SelectInput
          :options="[
            { value: 'all', label: 'All' },
            { value: 'completed', label: 'Completed' },
            { value: 'uncompleted', label: 'Uncompleted' },
            { value: 'favorites', label: 'Favorites' },
          ]"
          :value="selectedStatus"
          @input="inputSelectedStatus"
        />
      </div>
      <div class="to-do__header__section">
        <span class="to-do__header__section__title">User ID</span>
        <SelectInput
          :options="userIds"
          :value="selectedUserId"
          @input="inputSelectedUserId"
        />
      </div>
      <div class="to-do__header__section">
        <label for="search">Search</label>
        <input
          id="search"
          placeholder="Search to do name"
          type="text"
          v-model="searchTitle"
        />
      </div>
    </div>
    <ul class="to-do__list">
      <li
        class="to-do__list__item"
        v-for="todo in filteredTodosWithBackgroundColor"
        :key="todo.id"
        :class="{ completed: todo.completed }"
      >
        <span v-html="todo.highlightedTitle"></span>
        <button
          class="to-do__list__item__favorite"
          @click="toggleFavorite(todo.id)"
          :style="{ backgroundColor: todo.backgroundColor }"
        >
          <img
            class="to-do__list__item__favorite__img"
            :class="{ active: todo.isFavorite }"
            src="../assets/favorite.png"
          />
        </button>
      </li>
    </ul>
  </div>
</template>

<script>
import SelectInput from "@/components/Select";
export default {
  name: "ToDoList",
  components: {
    SelectInput,
  },
  data() {
    return {
      todos: [],
      selectedStatus: "all",
      selectedUserId: "all",
      favoriteIds: JSON.parse(localStorage.getItem("favoriteIds") || "[]"),
      searchTitle: "",
    };
  },
  created() {
    fetch("https://jsonplaceholder.typicode.com/todos")
      .then((response) => {
        if (!response.ok) {
          throw new Error("Network response was not ok");
        }
        return response.json();
      })
      .then((data) => {
        this.todos = data;
      })
      .catch((error) => {
        console.error("Error:", error);
      });
  },
  computed: {
    userIds() {
      const ids = new Set(this.todos.map((todo) => todo.userId));
      return [
        { value: "all", label: "All" },
        ...Array.from(ids).map((id) => ({ value: id, label: id })),
      ];
    },
    searchTitleRegex() {
      return new RegExp(this.searchTitle, "gi");
    },
    filteredTodosWithBackgroundColor() {
      return this.filteredTodos.map((todo) => {
        const highlightedTitle = todo.title.replace(
          this.searchTitleRegex,
          (match) => {
            return `<span style="background-color: #ffff00">${match}</span>`;
          }
        );
        return {
          ...todo,
          backgroundColor: todo.isFavorite ? "#121116" : "#ffffff",
          highlightedTitle,
        };
      });
    },
    favoriteTodos() {
      return this.todos.filter((todo) => {
        return this.favoriteIds.includes(todo.id);
      });
    },
    filteredTodos() {
      let filtered = this.todos;
      if (this.selectedStatus !== "all") {
        const status =
          this.selectedStatus === "favorites"
            ? true
            : this.selectedStatus === "completed";
        filtered = filtered.filter((todo) => todo.completed === status);
      }
      if (this.selectedUserId !== "all") {
        filtered = filtered.filter(
          (todo) => todo.userId === Number(this.selectedUserId)
        );
      }
      if (this.selectedStatus === "favorites") {
        filtered = [...this.favoriteTodos];
      }
      filtered = filtered.filter((todo) => {
        return todo.title
          .toLowerCase()
          .includes(this.searchTitle.toLowerCase());
      });
      filtered = filtered.map((todo) => {
        return {
          ...todo,
          isFavorite: this.favoriteIds.includes(todo.id),
          backgroundColor: this.favoriteIds.includes(todo.id)
            ? "#121116"
            : "#ffffff",
        };
      });
      return filtered;
    },
  },
  methods: {
    toggleFavorite(id) {
      const index = this.favoriteIds.indexOf(id);
      if (index === -1) {
        this.favoriteIds.push(id);
      } else {
        this.favoriteIds.splice(index, 1);
      }
      localStorage.setItem("favoriteIds", JSON.stringify(this.favoriteIds));
    },
    inputSelectedStatus(value) {
      this.selectedStatus = value;
    },
    inputSelectedUserId(value) {
      this.selectedUserId = value;
    },
  },
};
</script>

<style lang="scss">
.to-do {
  margin-top: 30px;
  .to-do__title {
    text-transform: uppercase;
    margin-bottom: 20px;
  }
  .to-do__header {
    display: flex;
    justify-content: space-between;
    column-gap: 15px;
    .to-do__header__section {
      display: flex;
      flex-direction: column;
      width: 33%;
      .to-do__header__section__title {
        font-size: 14px;
        color: #121116;
      }
      label {
        font-size: 14px;
        color: #121116;
      }
      input {
        width: 100%;
        box-sizing: border-box;
        padding: 0 12px 0 12px;
        height: 45px;
        border-radius: 5px;
        margin: 5px 0;
        border: 1px solid #121116;
        transition: all 0.1s linear;
        &:focus {
          outline: none !important;
          border: 1px solid #cececf;
        }
      }
    }
  }
  .to-do__list {
    max-height: calc(100vh - 300px);
    overflow-y: scroll;
    padding: 10px 5px 20px 0;
    &::-webkit-scrollbar {
      background-color: black !important;
      -webkit-appearance: none !important;
      width: 4px !important;
    }

    &::-webkit-scrollbar-thumb {
      border-radius: 2px !important;
      background-color: #e3e1ef !important;
    }
    .to-do__list__item {
      display: flex;
      align-items: center;
      justify-content: space-between;
      padding: 0 15px;
      height: 45px;
      border: 1px solid #cececf;
      border-radius: 5px;
      background-color: rgba(206, 206, 207, 0.1);
      margin-bottom: 10px;
      &.completed {
        background-color: rgba(0, 128, 0, 0.2);
      }
      .to-do__list__item__favorite {
        cursor: pointer;
        transition: all 0.1s linear;
        border-radius: 50%;
        display: flex;
        align-items: center;
        justify-content: center;
        border: 1px solid transparent;
        width: 35px;
        height: 35px;
        &:hover {
          background-color: rgba(19, 18, 23, 0.19) !important;
        }
        .to-do__list__item__favorite__img {
          width: 19px;
          height: 18px;
          &.active {
            filter: invert(100%) sepia(100%) saturate(0%) hue-rotate(160deg)
              brightness(100%) contrast(104%);
          }
        }
      }
    }
  }
}
</style>
