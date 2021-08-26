<template>
  <div class="wrapper-content wrapper-content--fixed">
    <section>
      <div class="container">
        <table>
          <thead>
            <tr>
              <th
                @click="sort('first_name')"
                :class="{ active: currentSort === 'first_name' }"
              >
                First name &#8595;
              </th>
              <th
                @click="sort('last_name')"
                :class="{ active: currentSort === 'last_name' }"
              >
                Last name &#8595;
              </th>
              <th
                @click="sort('email')"
                :class="{ active: currentSort === 'email' }"
              >
                Email &#8595;
              </th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="user in usersSort" :key="user.id">
              <td>
                <img :src="user.avatar" :alt="user.email" />
                <span>{{ user.first_name }}</span>
              </td>
              <td>{{ user.last_name }}</td>
              <td>{{ user.email }}</td>
            </tr>
          </tbody>
        </table>
        <p>Debug</p>
        <p>sort: {{ currentSort }}, dir: {{ currentSortDir }}</p>
        <span>
          page: {{ this.page.current }}, length: {{ this.page.length }}
        </span>
      </div>
    </section>
    <section>
      <div class="container">
        <div class="button-list">
          <button class="btn btnPrimary" @click="prevPage">&#8592;</button>
          <button class="btn btnPrimary" @click="nextPage">&#8594;</button>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      users: [],
      currentSort: "first_name",
      currentSortDir: "asc",
      page: {
        current: 1,
        length: 4,
      },
    };
  },
  created() {
    axios
      .get("https://reqres.in/api/users")
      .then((response) => {
        this.users = response.data.data;
      })
      .catch((error) => {
        console.log(error);
      });
  },
  computed: {
    usersSort() {
      return this.users
        .sort((a, b) => {
          let mod = 1;

          if (this.currentSortDir === "desc") mod = -1;
          if (a[this.currentSort] < b[this.currentSort]) return -1 * mod;
          if (a[this.currentSort] > b[this.currentSort]) return 1 * mod;
          return 0;
        })
        .filter((row, index) => {
          let start = (this.page.current - 1) * this.page.length;
          let end = this.page.current * this.page.length;
          if (index >= start && index < end) return true;
        });
    },
  },
  methods: {
    sort(e) {
      if (e === this.currentSort) {
        this.currentSortDir = this.currentSortDir === "asc" ? "desc" : "asc";
      }
      this.currentSort = e;
    },
    // Pagination
    prevPage() {
      if (this.page.current > 1) this.page.current -= 1;
    },
    nextPage() {
      if (this.page.current * this.page.length < this.users.length)
        this.page.current += 1;
    },
  },
};
</script>

<style lang="scss" scoped>
img {
  width: 60px;
  height: auto;
  margin-right: 16px;
  border-radius: 50%;
}

.button-list {
  width: 100%;
  text-align: center;

  .btn {
    margin: 0 20px;
    border-radius: 60px;
  }
}

.active {
  font-weight: 700;
  color: #494ce8;
}
</style>