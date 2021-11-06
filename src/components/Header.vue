<template>
  <header class="header">
    <div class="app-container">
      <div class="header-inner">
        <a href="/" class="header-logo">Github Api List</a>
        <nav class="header-nav">
          <a href="#" class="header-nav-link">Link</a>
          <a href="#" class="header-nav-link">Link</a>
          <a href="#" class="header-nav-link">Link</a>
          <a href="#" class="header-nav-link">Link</a>
        </nav>
      </div>
    </div>
  </header>
</template>

<script>
export default {
  name: "Header",
};
</script>

<style lang="scss" scoped>
@import "../styles/variables";

.header {
  box-shadow: 0px -5px 5px -5px rgba(34, 60, 80, 0.6) inset;
  min-height: 100px;
  color: $font-header;
  margin-bottom: $space-xl;
  display: flex;
  background-color: $item-bcg;

  &-inner {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: $space-xl 0 $space-xl;
    flex: 1;
  }

  &-logo {
    text-transform: uppercase;
    font-weight: bold;
    font-size: 32px;
    color: $font-header;
  }

  &-nav {
    &-link {
      color: #000;
      transition: color .3s, background-color .3s;
      padding: $space-m;
      margin-right: $space-m;
      background-color: transparent;

      &:last-child {
        margin-right: 0;
      }

      &:hover {
        color: $font-light;
        background-color: $font-header;
      }
    }
  }
}
</style>
