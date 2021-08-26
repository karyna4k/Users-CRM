<template>
  <div class="wrapper-content wrapper-content--fixed">
    <section>
      <div class="container">
        <table>
          <thead>
            <tr>
              <th @click="sort('first_name')">First name</th>
              <th @click="sort('last_name')">Last name</th>
              <th @click="sort('email')">Email</th>
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
      return this.users.sort((a, b) => {
        let mod = 1;

        if (this.currentSortDir === "desc") mod = -1;
        if (a[this.currentSort] < b[this.currentSort]) return -1 * mod;
        if (a[this.currentSort] > b[this.currentSort]) return 1 * mod;
        return 0;
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
</style>