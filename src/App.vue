<template>
  <div id="app">
    <navigate />
    
    <router-view
    style="padding-top: 60px;margin-bottom: -60px;"
    />
   
    <!-- <HostInfoPage></HostInfoPage> -->
  </div>
</template>

<script>
import navigate from '@/components/navigate.vue';
import HostInfoPage from "@/views/HostInfoPage";
export default {
  name: 'App',
  components: {
    HostInfoPage,
    navigate
  }
}
</script>


<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
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