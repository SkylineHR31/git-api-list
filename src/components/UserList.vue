<template>
  <div class="user-list-wrapper">
    <ul class="user-list">
      <li
        class="user-list-item"
        v-bind:v-for="i in currentPageData"
        v-bind:key="i.id"
      >
        <div>
          {{ i.id }}
          {{ i.name }}
          {{ i.owner }}
          {{ i.avatar }}
          {{ i.reposLink }}
          {{ i.watchers }}
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
import axios from "axios";
const API_URL = "https://api.github.com/search/repositories?q=";

export default {
  name: "UserList",
  data() {
    return {
      perPage: 30,
      language: "javascript",
      currentPage: 1,
      totalPages: 0,
      currentPageData: [],
    };
  },
  methods: {
    async getDataFromApi() {
      try {
        const response = await axios.get(
          `${API_URL}language:${this.language}&per_page=${this.perPage}&page=${this.currentPage}`
        );
        setPageData(response.data.items);
        this.pageCount(response.data.total_count, this.perPage);
        console.log(response);
      } catch (error) {
        console.log(error);
      }
    },

    pageCount(repositories, perPage) {
      this.totalPages = Math.ceil(repositories / perPage);
    },

    nextPage() {
      this.currentPage++;
    },
    prevPage() {
      this.currentPage--;
    },
  },
  mounted() {},
  created() {
    this.getDataFromApi();
  },
  computed: {
    setPageData: function (items) {
      items.forEach((element) => {
        this.currentPageData.push({
          id: element.id,
          name: element.full_name,
          owner: element.owner.login,
          avatar: element.owner.avatar_url,
          reposLink: element.html_url,
          watchers: element.watchers,
        });
      });
    },
  },
};
</script>

<style lang="scss" scoped>
@import "../styles/variables";

.user-list {
}
</style>
