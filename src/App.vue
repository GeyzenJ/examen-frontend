<template>
  <div id="app">
    <!-- navigatie bar -->
     <ul>
      <li v-for="(page, index) in pages" :key="index" @click="changePage(page)">
        {{ page }}
      </li>
     </ul>
    
    <!-- om onderscheid tussen nav en content -->
     <!-- nog aanpassen naar fatsoenlijk (css) -->
     <hr>
     <!-- Hierna content Pages dmv content in div-->
    <!-- roept de content van de page op -->
    <div>
      <PageHome v-if="activePage == 'Home'" />
      <PageLogin v-if="activePage == 'Login'" />
      <PageUserInfo v-if="activePage == 'UserInfo'" />
    </div>

  </div>
</template>

<script>
import PageHome from './components/PageHome.vue';
import PageLogin from './components/PageLogin.vue';
import PageUserInfo from './components/PageUserInfo.vue';

export default {
  name: 'App',
  components: {
    PageHome,
    PageLogin,
    PageUserInfo
  },
  data() {
   return { 
    activePage: 'Home',
    pages: ['Home', 'Login', 'UserInfo']
    }
  },
  methods: {
    changePage(page) {
      this.activePage = page;
    }
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
  margin-top: 60px;
}
</style>
