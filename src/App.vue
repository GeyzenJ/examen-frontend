<template>
  <div id="app">
    <!-- navigatie bar -->    
<!-- niet ingelogd -->
    <ul v-if="!isIngelogd">
      <li v-for="(page, index) in pagesNietIngelogd" :key="index" @click="changePage(page)">
        {{ page }}
      </li>
     </ul>
    <!-- ingelogd -->
     <!-- GEEN admin -->
    <div v-if="isIngelogd">
      <ul v-if="this.isAdmin == false">
      <li v-for="(page, index) in pagesIngelogd" :key="index" @click="changePage(page)">
        {{ page }}
      </li>
     </ul>
    
     <!-- ingelogd WEl admin -->
      <ul v-if="this.isAdmin == true">
          <li v-for="(page, index) in pagesBeheer" :key="index" @click="changePage(page)">
            {{ page }}
          </li>
        </ul>
    </div>


    <!-- om onderscheid tussen nav en content -->
     <!-- nog aanpassen naar fatsoenlijk (css) -->
     <hr>
     <!-- Hierna content Pages dmv content in div-->
    <!-- roept de content van de page op -->
    <div>
      <PageHome v-if="activePage == 'Home'" />
      <PageLogin v-if="activePage == 'Login' || activePage == 'Log uit'" @login="handleLogin" @logout="handleLogout"/>
      <PageUserInfo v-if="activePage == 'Gebruikers informatie'" />
      <PageOverzichtBoekingen v-if="activePage == 'Overzicht boekingen'"/>
      <PageBoeken v-if="activePage == 'Campingplaats boeken'" />
      <PageOverzichtBeheerCampings v-if="activePage == 'Overzicht campings'" />
      <PageUpdateCamping v-if="activePage == 'Update gegevens campings'" />
      <PageAddCamping v-if="activePage == 'Camping toevoegen'" />
      <PageZoekCamping v-if="activePage == 'Zoek een camping'" />
    </div>

  </div>
</template>

<script>
import PageHome from './components/PageHome.vue';
import PageLogin from './components/PageLogin.vue';
import PageUserInfo from './components/PageUserInfo.vue';
import PageOverzichtBoekingen from './components/PageOverzichtBoekingen.vue';
import PageBoeken from './components/PageBoeken.vue';
import PageOverzichtBeheerCampings from './components/PageOverzichtBeheerCampings.vue';
import PageUpdateCamping from './components/PageUpdateCamping.vue';
import PageAddCamping from './components/PageAddCamping.vue';
import PageZoekCamping from './components/PageZoekCamping.vue';

export default {
  name: 'App',
  components: {
    PageHome,
    PageLogin,
    PageUserInfo,
    PageBoeken,
    PageOverzichtBoekingen,
    PageOverzichtBeheerCampings,
    PageUpdateCamping,
    PageAddCamping,
    PageZoekCamping
  },
  data() {
   return { 
    activePage: 'Home',
    pagesIngelogd: ['Home', 'Log uit', 'Gebruikers informatie', 'Zoek een camping',
           'Campingplaats boeken', 'Overzicht boekingen'],
    pagesNietIngelogd: ['Home', 'Login', 'Zoek een camping'],
    pagesBeheer: ['Home', 'Log uit', 'Gebruikers informatie', 'Zoek een camping',
           'Overzicht campings','Update gegevens campings', 'Camping toevoegen'],
    userId: null,
    isIngelogd: false,
    isAdmin: false
    }
  },
  methods: {
    changePage(page) {
      this.activePage = page;
    },
        handleLogin(userId, isAdmin) {
          this.userId = userId;
          if (isAdmin == 1) {
            this.isAdmin = true;
          }          
          this.isIngelogd = true;
        },
        handleLogout() {
          this.userId = null;
          this.isAdmin = false;
          this.isIngelogd = false;
        }
      },
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
