<template>
  <v-app>
    <v-app-bar app color="blue">
      <svg style="width:24px;height:24px" viewBox="0 0 24 24">
    <path fill="currentColor" d="M23 11.5L19.95 10.37C19.69 9.22 19.04 8.56 19.04 8.56C17.4 6.92 14.75 6.92 13.11 8.56L11.63 10.04L5 3C4 7 5 11 7.45 14.22L2 19.5C2 19.5 10.89 21.5 16.07 17.45C18.83 15.29 19.45 14.03 19.84 12.7L23 11.5M17.71 11.72C17.32 12.11 16.68 12.11 16.29 11.72C15.9 11.33 15.9 10.7 16.29 10.31C16.68 9.92 17.32 9.92 17.71 10.31C18.1 10.7 18.1 11.33 17.71 11.72Z" />
</svg>
      <v-toolbar-title style="cursor: pointer" @click="$router.push('/')" >Eagle Finance Services</v-toolbar-title>
      
      <v-spacer></v-spacer>
      <div class="hidden-xs-only">
        <v-btn
          v-for="item in menu"
          :key="item.title"
          :to="item.url"
          flat
          text 
          rounded
          dark
        >{{ item.title }}</v-btn>
        <v-btn flat dark text rounded v-if="!authenticated"
                @click="login">Log in
        </v-btn>
        <v-btn flat dark text rounded v-if="!authenticated"
                @click="register">Register
        </v-btn>
        <v-btn flat dark text rounded v-if="authenticated"
                @click="logout">Log Out
        </v-btn>
      </div>
      <div class="hidden-sm-and-up">
      <v-menu bottom left>
        <template v-slot:activator="{ on, attrs }">
          <v-btn
            dark
            icon
            v-bind="attrs"
            v-on="on"
          >
            <v-icon>mdi-dots-vertical</v-icon>
          </v-btn>
        </template>
        <v-list>
          <v-list-item  v-for="item in menu"
            :key="item.title"
            :to="item.url"
          >{{ item.title }}</v-list-item>
          <v-list-item v-if="!authenticated"
          @click="login">Log in</v-list-item>
          <v-list-item v-if="authenticated"
          @click="logout"> <v-icon dark left>
          mdiLogout
        </v-icon>Log out
      
      </v-list-item>
        </v-list>
      </v-menu>
      </div>
    </v-app-bar>
    <v-content>
      <router-view/>
    </v-content>
    <v-footer padless color="primary lighten-1">
    <v-col
      class="text-center"
      cols="12"
    >
      {{ new Date().getFullYear() }} — <strong>EFS</strong>
    </v-col>
  </v-footer>
  </v-app>

</template>

<script>
  import router from './router';
  import {APIService} from './http/APIService';
  const apiService = new APIService();

  export default {
    name: 'App',
    data: () => ({
      authenticated: false,
      dialog: false,
      menu: [
        { title: 'Home', url:"/"},
        { title: 'Customers', url:"/customer-list" },
        { title: 'Stocks', url:"/stock-list" },
        { title: 'Investments', url:"/investment-list" },
      ]
    }),

    mounted() {
      apiService.getCustomerList().then(response => {
        this.authenticated = true;
      }).catch(error => {
        if (error.response.status === 401) {
          localStorage.removeItem('isAuthenticates');
          localStorage.removeItem('log_user');
          localStorage.removeItem('token');
          this.authenticated = false;
        }
      });
      console.log('this.authenticated--'+this.authenticated);
    },

    methods: {
      logout() {
        localStorage.removeItem('isAuthenticates');
        localStorage.removeItem('log_user');
        localStorage.removeItem('token');
        this.authenticated = false;
        // router.push('/');
        window.location = "/"
      },
      login() {
        router.push("/auth");
      },
      register() {        
  router.push('/register');
},

    }
  };

</script>	

<style>
.v-data-table > .v-data-table__wrapper > table{
  background-color: #a0daeba9 !important;
  border-bottom: none !important;
  font-family: 'Lucida Sans', 'Lucida Sans Regular', 'Lucida Grande', 'Lucida Sans Unicode', Geneva, Verdana, sans-serif;

}
.v-data-table > .v-data-table__wrapper > table > tbody > tr:hover{
  background:white !important;
}
.v-data-table > .v-data-footer{
    background-color: #a0daeba9 !important;

}
.v-data-table > .v-data-table__wrapper > table > tbody > tr > th,
.v-data-table > .v-data-table__wrapper > table > thead > tr > th {
   font-size: 15px !important;
   font-family: 'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif;
   background: #5facc4;
}

</style>