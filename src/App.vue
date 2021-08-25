<template>
  <div class="wrapper">
    <!-- <header></header> -->
    <header>
      <div class="navbar">
        <div class="container">
          <div class="navbar-content">
            <div class="logo">Users CRM</div>
            <nav>
              <ul class="navbar-list">
                <li class="navbar-item" v-for="link in links" :key="link.title">
                  <router-link
                    class="navbar-link"
                    :title="link.title"
                    :to="link.url"
                    >{{ link.title }}</router-link
                  >
                </li>
              </ul>
            </nav>
          </div>
        </div>
      </div>
    </header>

    <router-view></router-view>

    <!-- <footer></footer> -->
  </div>
</template>

<script>
export default {
  data() {
    return {
      links: [
        { title: "Home", url: "/" },
        { title: "Users", url: "/users" },
      ],
    };
  },
};
</script>

<style lang="scss">
.navbar-link {
  &.router-link-exact-active {
    color: #494ce8;
    font-weight: 700;
  }
}
</style>
