<template>
  <div class="user">
    <Loader v-if="loader" class="user__loader" />
    <div v-else class="user__content">
      <div class="user__content__header">
        <h1>PROFILE</h1>
        <button
          @click="$router.push({ name: 'home' })"
          class="user__content__header__button"
        >
          RE-LOGIN
        </button>
      </div>
      <UserInformation :user="user" />
      <ToDoList />
    </div>
  </div>
</template>

<script>
import Loader from "@/components/Loader";
import UserInformation from "@/components/UserInformation";
import ToDoList from "@/components/ToDoList";
export default {
  name: "users",
  components: {
    Loader,
    UserInformation,
    ToDoList,
  },
  data() {
    return {
      user: {},
      loader: true,
    };
  },
  mounted() {
    this.getUser();
  },
  methods: {
    getUser() {
      const url = `https://jsonplaceholder.typicode.com/users`;
      fetch(url)
        .then((response) => response.json())
        .then((data) => {
          this.user = data.find((user) => {
            return user.id === this.$route.params.id;
          });
          this.loader = false;
        });
    },
  },
};
</script>

<style lang="scss">
.user {
  padding: 40px;
  .user__loader {
    position: fixed;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
  }
  .user__content__header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    .user__content__header__button {
      padding: 8px 12px;
      border-radius: 5px;
      cursor: pointer;
      background-color: transparent;
      border: 1px solid #121116;
      color: #121116;
      transition: all 0.1s linear;
      &:hover {
        background-color: #121116;
        color: white;
      }
    }
  }
}
</style>
