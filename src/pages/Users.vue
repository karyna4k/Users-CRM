<template>
  <div class="wrapper-content wrapper-content--fixed">
    <section>
      <div class="container">
        <table>
          <thead>
            <tr>
              <th>First name</th>
              <th>Last name</th>
              <th>Email</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="user in users" :key="user.id">
              <td>
                <img :src="user.avatar" :alt="user.email">
                <span>{{ user.first_name }}</span>
              </td>
              <td>{{ user.last_name }}</td>
              <td>{{ user.email }}</td>
            </tr>
          </tbody>
        </table>
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