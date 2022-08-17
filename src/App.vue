<template>
  <nav>
    <router-link to="/">Books</router-link> |
    <router-link to="/about">Movies</router-link>
  </nav>
  <router-view/>
</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
   text-align: center;
  /*color: #2c3e50; */
  background-color: #0f1011;
  color: #fff;
  height: 100vh;
}
* {
  margin: 0px;
  padding: 0px;
}

nav {
  padding: 30px;
}

nav a {
  font-weight: bold;
  color: #2c3e50;
}

nav a.router-link-exact-active {
  /* color: #42b983; */
  color: #e70a0a;
}

</style>
