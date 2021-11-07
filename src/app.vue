<template>
  <div class="app-layout">
    <Header></Header>
    <main class="content">
      <div class="app-container">
        <UserList></UserList>
      </div>
    </main>
    <Footer></Footer>
  </div>
</template>

<script>
import Header from "@/components/Header";
import Footer from "@/components/Footer";
import UserList from "@/components/UserList";

export default {
  name: "App",
  components: {
    Header,
    Footer,
    UserList,
  },
};
</script>

<style lang="scss">
@import "./styles/layout";
@import "./styles/variables";

.content {
  flex: 1;
  padding-bottom: $space-xl;
  border-bottom: 1px solid $font-gray;
}
</style>
