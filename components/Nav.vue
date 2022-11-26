<template>
    <nav class="navbar navbar-expand-md navbar-dark bg-dark mb-4">
      <div class="container-fluid" >
  
        <div>
          <ul class="navbar-nav me-auto mb-2 mb-md-0" v-if="!auth">
            <li class="nav-item">
              <router-link to="/login" class="nav-link">Login</router-link>
            </li>
            <li class="nav-item">
              <router-link to="/register" class="nav-link">Register</router-link>
            </li>
          </ul>
  
          <ul class="navbar-nav me-auto mb-2 mb-md-0" v-if="auth">
            <li class="nav-item">
              <a href="#" class="nav-link" @click="logout">Logout</a>
            </li>
             <li class="nav-item">
                <router-link to="/home" class="nav-link">Home</router-link>
            </li>
             <li class="nav-item">
                <router-link to="/addpost" class="nav-link">Add Post</router-link>
            </li>
             <li class="nav-item">
                <router-link to="/users-name" class="nav-link">All Users Info</router-link>
            </li>
             <li class="nav-item">
                <router-link to="/profile" class="nav-link">Profile</router-link>
            </li>
          </ul>
        </div>
  
      </div>
    </nav>
  </template>
  
  
  <style>
          @import url('https://fonts.googleapis.com/css2?family=Roboto:wght@300&display=swap');
          @import url('https://fonts.googleapis.com/css2?family=Lobster&display=swap');
          *{
              margin: 0;
              padding: 0;
              box-sizing: border-box;
          }
          nav{
              display: flex;
              width: 100%;
              height: 60px;
              justify-content: space-between;
              align-items: center;
              background: linear-gradient(to left,rgb(118, 84, 173),rgb(150, 77, 59));
          }
          .logo{
              font-family: 'Lobster', cursive;
              font-weight: 200;
              font-size:20px;
              color: white;
              margin-left: 50px;
              letter-spacing: 2px;
          }
          nav ul {
              display: flex;
              list-style: none;
          }
          nav li{
              width: 150px;
          }
          nav a{
              text-decoration: none;
              color: white;
              letter-spacing: 1px;
              font-size: 15px;
              font-family: 'Roboto', sans-serif;
          }
          @media (max-width:700px) {
              nav .logo{
                  margin-left: 30px;
                  font-size: 16px;
              }
              nav li{
                  width: 100px;
              }
          }
      </style>
  
  
  
  <script lang="ts">
  import {computed} from 'vue';
  import {useStore} from "vuex";
  import {useRouter} from "vue-router";
  
  export default {
    name: "Nav",
    setup() {
      const router = useRouter();
      const store = useStore();
  
      const auth = computed(() => store.state.authenticated)
  
      const logout = async () => {
                 document.cookie = "jwt=; expires=Thu, 01 Jan 1970 00:00:00 GMT";
                 router.push('/');
                 store.dispatch('setAuth', false);
                  
      }
  
      return {
        auth,
        logout
      }
    }
  }
  </script>