<template>
  <div class="user-info">
    <h4 class="user-info__title">{{ user.name }}</h4>
    <span class="user-info__small-text">Email:{{ " " + user.email }}</span>
    <span class="user-info__small-text">Phone:{{ " " + user.phone }}</span>
    <span class="user-info__small-text">Website:{{ " " + user.website }}</span>
    <h4 class="user-info__subtitle">Address</h4>
    <div class="user-info__section">
      <div class="user-info__section__element">
        <span class="user-info__section__element__title">City</span>
        {{ " " + user.address.city }}
      </div>
      <div class="user-info__section__element">
        <span class="user-info__section__element__title">Street</span>
        {{ " " + user.address.street }}
      </div>
      <div class="user-info__section__element">
        <span class="user-info__section__element__title">Suite</span>
        {{ " " + user.address.suite }}
      </div>
      <div class="user-info__section__element">
        <span class="user-info__section__element__title">Zipcode</span>
        {{ " " + user.address.zipcode }}
      </div>
    </div>
    <h4 class="user-info__subtitle">Company</h4>
    <div class="user-info__section">
      <div class="user-info__section__element">
        <span class="user-info__section__element__title">Name</span>
        {{ " " + user.company.name }}
      </div>
      <div class="user-info__section__element">
        <span class="user-info__section__element__title">Catch phrase</span>
        {{ " " + user.company.catchPhrase }}
      </div>
      <div class="user-info__section__element">
        <span class="user-info__section__element__title">BS</span>
        {{ " " + user.company.bs }}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "User",
  props: {
    user: {
      type: Object,
      default: {},
    },
  },
};
</script>

<style lang="scss">
.user-info {
  display: flex;
  flex-direction: column;
  margin-top: 30px;
  padding-bottom: 40px;
  border-bottom: 1px solid #cececf;
  .user-info__title {
    margin-bottom: 10px;
  }
  .user-info__small-text {
    font-size: 14px;
    color: #656565;
    margin-bottom: 5px;
  }
  .user-info__subtitle {
    margin: 10px 0;
  }
  .user-info__section {
    display: flex;
    align-items: center;
    column-gap: 50px;
    .user-info__section__element {
      display: flex;
      flex-direction: column;
      font-size: 14px;
      color: #656565;
      .user-info__section__element__title {
        font-size: 12px;
        font-weight: 700;
        color: #121116;
      }
    }
  }
}
</style>
