<template>
  <footer class="footer">
    <div class="app-container">
      <div class="footer-inner">
        <div class="copy">
          Copyright © Alexandr Furnmanov 2033 Все права
          <strong>НЕ</strong> защищены :D
        </div>
      </div>
    </div>
  </footer>
</template>

<script>
export default {
  name: "Footer",
};
</script>

<style lang="scss" scoped>
@import "../styles/variables";

.footer {
  min-height: 50px;
  color: $font-header;
  display: flex;
  background-color: $item-bcg;

  &-inner {
    display: flex;
    align-items: center;
    padding: $space-m 0 $space-m;
    flex: 1;
  }
}
</style>
