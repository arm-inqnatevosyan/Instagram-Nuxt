<template>
    <nav>
        <div class="logo"><h1>Instagram</h1></div>
        <div class="icons">
             <NuxtLink to="/"><i class="fa-solid fa-house"></i></NuxtLink>
            <i class="fa-solid fa-magnifying-glass"></i>
            <NuxtLink to="/"><img src="./story/home.png" width="20" /></NuxtLink>
            <img src="./story/search.png" width="20" />
            <img src="./story/heart.png" width="20" />
        </div>
        <ul>
            <li><NuxtLink to="/">Home</NuxtLink></li>
            <li><NuxtLink to="/sign">Sign In</NuxtLink></li>
            <li><NuxtLink to="/registr">Register</NuxtLink></li>
            <li><NuxtLink to="/profile">Profile</NuxtLink></li>
        </ul>
    </nav>
  </template>
  
  
  <style scoped>
  @import url('https://fonts.googleapis.com/css2?family=Roboto:wght@300&display=swap');
nav{
    width:  80%;
    height: 70px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin: 0 auto !important;
    margin-left: 30px;
    margin-right: 30px;
}
ul{
    display: flex;
    list-style: none;
    align-items: center;
}
.logo{
    font-family: 'Lobster', cursive;
    width: 200px;
}
.icons{
    width: 160px;
    display: flex;
    justify-content: space-around;
    align-items: center;
    height: 20px;
}
.icons i{
    font-size: 21px;
}
li{
    margin-left: 30px;
}
a{
    text-decoration: none;
    font-family: 'Trebuchet MS', 'Lucida Sans Unicode', 'Lucida Grande', 'Lucida Sans', Arial, sans-serif;
    color: black;
}
  </style>
  
  
  
  <script >
  import {computed} from 'vue';
  import { useRouter, useStore } from '@nuxtjs/composition-api'
  export default {
    name: "NavVue",
    setup() {
      const router = useRouter();
      const store = useStore();
  
      const auth = computed(() => store.state.authenticated)
  
      const logout = () => {
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