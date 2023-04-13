<template>
  <header class="header">
    <div class="container">
      <div class="header__wrapper">
        <router-link class="header__logo" to="/">
          <img src="@/assets/images/logo.svg" alt="Система таблиц" width="40" height="35">
          <span>Система таблиц</span></router-link>
        <div class="header__user">
          <span class="header__user-name">Светлана</span>
          <span class="header__user-post">Сотрудник</span>
          <img class="header__user-photo" src="@/assets/images/user.png" alt="user" width="44" height="44">
        </div>
      </div>

    </div>

  </header>
</template>

<script>
export default {

}
</script>

<style lang="scss">
.header {
  background-color: $white;
  padding: 25px 0;
  position: sticky;
  top: 0;
z-index: 10;
  &__wrapper {
    display: flex;
    align-items: center;
    justify-content: space-between;
  }

  &__logo {
    max-width: 130px;
    display: flex;
    align-items: center;
    gap: 10px;
    color: $blue;
    text-transform: uppercase;
    @extend %text-800-24;
    font-size: 16px;
    line-height: 18px;
  }

  &__user {
    display: grid;
    grid-template-columns: auto auto;
    column-gap: 10px;

    &-name,
    &-post {
      grid-column: 1;
      text-align: right;
    }

    &-name {
      @extend %text-700-14;
      color: $blue;
    }

    &-post {
      @extend %text-400-11;
      font-size: 14px;
      line-height: 17px;
      color: $medium;
    }

    &-photo {
      border-radius: 50%;
      grid-column: 2;
      grid-row: 1/3;
    }
  }
}
</style>