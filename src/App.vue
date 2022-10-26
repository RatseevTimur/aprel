<template>
  <div id="app">
    <nav>
      <router-link to="/">Products</router-link> &#128230;<br>
      <router-link to="/about">Profile</router-link> &#128100;
    </nav>
    <router-view />
  </div>
</template>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

nav {
  position: relative;
  text-align: end;
  width: 90%;
  padding: 15px;

  a {
    font-weight: bold;
    color: #2c3e50;

    &.router-link-exact-active {
      color: #42b983;
    }
  }
}
</style>
