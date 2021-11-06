<template>
  <div class="user-list-wrapper">
    <p v-show="isServerError" class="error">Sorry, but server is tired :D</p>
    <ul v-show="!isServerError" class="user-list">
      <li class="user-list-item" v-for="i in currentPageData" :key="i.id">
        <div class="user-card">
          <div class="user-card-avatar">
            <img :src="i.avatar" :alt="i.owner" />
          </div>
          <div class="user-card-details">
            <a :href="i.reposLink" class="user-card-link">{{ i.reposLink }}</a>
            <div class="user-card-summary">
              <span class="title">Summary</span>
              <table class="user-card-summary-table">
                <thead>
                  <tr>
                    <th>Id</th>
                    <th>Repos name</th>
                    <th>Author</th>
                    <th>Watchers</th>
                  </tr>
                </thead>
                <tbody>
                  <tr>
                    <td>
                      {{ i.id }}
                    </td>
                    <td>
                      {{ i.name }}
                    </td>
                    <td>
                      {{ i.owner }}
                    </td>
                    <td>
                      {{ i.watchers }}
                    </td>
                  </tr>
                </tbody>
              </table>
            </div>
          </div>
        </div>
      </li>
    </ul>
    <div class="user-list-controls">
      <button
        :disabled="prevDisabled ? true : null"
        class="user-list-control"
        v-on:click="prevPage"
      >
        Prev
      </button>
      <button
        :disabled="nextDisabled ? true : null"
        class="user-list-control"
        v-on:click="nextPage"
      >
        Next
      </button>
    </div>
  </div>
</template>

<script>
import axios from "axios";
const API_URL = "https://api.github.com/search/repositories?q=";

export default {
  name: "UserList",
  data() {
    return {
      perPage: 10,
      language: "javascript",
      currentPage: 1,
      totalPages: 0,
      currentPageData: [],
      prevDisabled: true,
      nextDisabled: false,
      isServerError: false,
    };
  },
  methods: {
    setPageData(items) {
      let dataArr = [];
      items.forEach((element) => {
        dataArr.push({
          id: element.id,
          name: element.full_name,
          owner: element.owner.login,
          avatar: element.owner.avatar_url,
          reposLink: element.html_url,
          watchers: element.watchers,
        });
      });
      return dataArr;
    },

    async getDataFromApi() {
      try {
        const response = await axios.get(
          `${API_URL}language:${this.language}&per_page=${this.perPage}&page=${this.currentPage}`
        )
        this.currentPageData = this.setPageData(response.data.items);
        this.pageCount(response.data.total_count, this.perPage);
        console.log(response);
      } catch (error) {
        console.error(error);
        this.isServerError = true;
      }
    },

    pageCount(repositories, perPage) {
      this.totalPages = Math.ceil(repositories / perPage);
    },

    controlsValidate() {
      if (this.currentPage <= 1) {
        this.prevDisabled = true;
        this.nextDisabled = false;
      } else if (this.currentPage <= this.totalPages && this.currentPage > 1) {
        this.prevDisabled = false;
        this.nextDisabled = false;
      } else if (this.currentPage >= this.totalPages) {
        this.nextDisabled = true;
      }
      console.log(this.currentPage, this.buttonDisabled);
    },

    nextPage() {
      this.currentPage++;
      this.getDataFromApi();
      this.controlsValidate();
    },
    prevPage() {
      this.currentPage--;
      this.getDataFromApi();
      this.controlsValidate();
    },
  },
  created() {
    this.getDataFromApi();
  },
};
</script>

<style lang="scss" scoped>
@import "../styles/variables";

.user-list {
  list-style: none;
  padding: 0;

  &-controls {
    display: flex;
    align-items: center;
    justify-content: space-between;
  }

  &-control {
    padding: 0 $space-xl;
    min-height: 50px;
    background-color: $item-bcg;
    display: flex;
    align-items: center;
    justify-content: center;
    border: none;
    cursor: pointer;
    opacity: 1;
    transition: color 0.3s, background-color 0.3s, opacity 0.3s;

    &:hover {
      color: $font-light;
      background-color: $font-header;
    }

    &:disabled {
      cursor: not-allowed;
      background-color: $font-light;
      color: $font-gray;
      opacity: 0.5;
    }
  }
}

.user-card {
  display: flex;
  margin-bottom: $space-xl;
  box-shadow: 4px 4px 8px 0px rgba(34, 60, 80, 0.2);
  padding: $space-xl;
  background-color: $item-bcg;

  &-avatar {
    width: 200px;
    height: 200px;
    margin-right: $space-xl;
    border-radius: 50%;
    overflow: hidden;

    img {
      object-fit: cover;
      width: 100%;
      height: 100%;
    }
  }

  &-link {
    display: inline-block;
    margin-bottom: $space-xl;
    padding-left: $space-l;
  }

  &-summary {
    .title {
      font-size: 24px;
      padding-left: $space-l;
    }

    &-table {
      border-spacing: $space-l;
    }
  }
}

.error {
  color: red;
  font-size: 24px;
  font-weight: bold;
  text-align: center;
  padding: $space-l 0;
}
</style>
