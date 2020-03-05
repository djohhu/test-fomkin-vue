<template>
  <footer class="footer">
    <div class="container">
      <div class="footer__wrapper">
        <div class="footer__copyright">
          © 2018-2019
        </div>
      </div>
    </div>
  </footer>
</template>

<script>
    export default {
        name: "Footer"
    }
</script>

<style lang="scss" scoped>
  @import "../assets/scss/variables";
  .footer {
    &__wrapper {
      padding-top: 12px;
      padding-bottom: 11px;
      border-top: 1px solid $color-grey-light;
    }
    &__copyright {
      font-family: $font-secondary;
      color: $color-grey-light;
      text-align: center;
      font-size: 14px;
    }
  }
</style>