<script>
  import Sidebar from '@/components/sidebar/Sidebar'
  import { sidebarWidth } from '@/components/sidebar/state'
  import { Tooltip } from "bootstrap";

  new Tooltip(document.body, {
    selector: "[data-bs-toggle='tooltip']",
  });
  export default {
    components: { Sidebar },
    setup() {
      return { sidebarWidth }
    }
  }
</script>

<template>
  <div class="container">
    <Sidebar />
    <router-view/>
    <!--<div :style="{ 'margin-left': sidebarWidth } style="background-color: rgba(0,0,0,0.2);"">
      <router-view />
    </div>-->
  </div>
  <link href='http://fonts.googleapis.com/css?family=Roboto:400,100,100italic,300,300italic,400italic,500,500italic,700,700italic,900italic,900' rel='stylesheet' type='text/css'>
</template>

<style>
  .container {
    padding-top: calc(5px + 2vh);
    padding-bottom: calc(5px + 2vh);
  }
  #app {
    font-family: 'Roboto';
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 50px;
    background-image: url("~@/assets/main_background.svg");
    background-size: cover;
    background-repeat: no-repeat;
    min-height: 95vh;
    -webkit-touch-callout: none;
    -webkit-user-select: none;
     -khtml-user-select: none;
       -moz-user-select: none;
        -ms-user-select: none;
            user-select: none;
  }

  #nav {
    padding: 30px;
  }

  #nav a {
    font-weight: bold;
    color: #2c3e50;
  }

  #nav a.router-link-exact-active {
    color: #42b983;
  }
</style>
