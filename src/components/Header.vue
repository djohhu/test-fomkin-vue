<template>
  <header class="header">
    <div class="container">
      <div class="header__wrapper">
        <h1 class="header__title">Test app</h1>
      </div>
    </div>
  </header>
</template>

<script>
    export default {
        name: "Header"
    }
</script>

<style lang="scss" scoped>
  @import "../assets/scss/variables";
  .header {
    &__wrapper{
      padding-top: 15px;
      padding-bottom: 30px;
    }

    &__title {
      font-size: 36px;
      line-height: 42px;
      text-transform: uppercase;
      text-align: center;
      font-family: $font-secondary;
    }
  }
</style>