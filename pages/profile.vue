<template>
    <html>
     <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.2.0/css/all.min.css" integrity="sha512-xh6O/CkQoPOWDdYTDqeRdPCVd1SpvCA9XXcUnZS2FmJNp1coAFzvtCN9BmamE+4aHK8yyUHUSCcJHgXloTyT2A==" crossorigin="anonymous" referrerpolicy="no-referrer" />
    <body>
        
      <div id="app">
    
    
        <div v-for="(todo, key) in todos" :key="key">
           
            <div class="card">
            <img :src="`${ todo.img[0]}`" alt="John" style="width:100%">
            <h1>{{ todo.name }}</h1>
            <input type="file" accept="image/*" @change="onChange" />
            <form v-on:submit.prevent="password">
             <input  id="text" type="text"  v-model="content" placeholder="Change Password" name="content" /><button id = 'send'>Change Password</button>
            </form>
            <p class="title">Email - {{ todo.email }}</p>
            <p>Social Media User</p>
            
        </div>
        </div>
    
    
      
      <div class="main">
        <div v-for="(post, key) in posts" :key="key">
         
            <img :src="`${ post.img[0]}`" height = '400' width = '400'>
    
    
            <div class="icons">
                <p><i class="fa-regular fa-heart"></i></p>
                <i class="fa-regular fa-comment" @click="f()"></i>
                
            </div>
            <p class='ed'>title: {{ post.title }}</p><br>
            <p class='ed'>description: {{ post.content }}</p><br>
    
            <div class='p' id = 'com'>
    
              
                <h6 id='com' v-for="comments in post.comments" :key="post.id">coments:{{ comments.content }}</h6>
                <form v-on:submit.prevent="submitForm(post._id)">
                <input  id="text" class="input" placeholder="Add Comment ..."  v-model="content"  name="content" />
                <button id = 'send'><i class="fa-regular fa-paper-plane"></i></button>
                </form>
     
            </div>   
    
             
        </div>
        </div>
      </div>
    </body>
    </html>
    </template>
    <script>
      import axios from "axios"; 
      import {reactive} from 'vue';
      import {useRouter} from "vue-router";
      import {useStore} from "vuex";
      export default {
        data() {
         const data = reactive({
           content: '',
           img: ''
        });
         return {
            todos:  [],
            posts: [],
            item:{
              image : null,
              imageUrl: null
          }
         };
        },
       methods: {
            
     async created() {
        try {
            
            const response = await fetch('http://localhost:8000/api/userss', {
              headers: {'Content-Type': 'application/json'},
              credentials: 'include'
            });
            const content = await response.json();
            let x = JSON.stringify(content)
            let mydata = JSON.parse(x);  
            this.todos = mydata
    
    
            const responses = await fetch(`http://localhost:8000/api/${mydata[0].name}/porfile_post`, {
              headers: {'Content-Type': 'application/json'},
              credentials: 'include'
            });
            const contents = await responses.json();
            let y = JSON.stringify(contents)
            let mydatas = JSON.parse(y);
            this.posts = mydatas
           
    
      
            for(var i = 0;i < this.posts.length; i++){
                         var Name = document.createElement("div");
                         var Description = document.createElement("div");
                         var Author = document.createElement("div");
                         const url = `http://localhost:8000/api/${mydatas[i].img[0]}/post_image`
        
                         const options = {
                            method: "GET"
                         }
        
                         let response = await fetch(url, options)
                         const imageBlob = await response.blob()
                         const imageObjectURL = URL.createObjectURL(imageBlob);
    
                         this.posts[i].img[0] = (imageObjectURL)
                         
            }
            for(var i = 0;i < this.todos.length; i++){
                         var Name = document.createElement("div");
                         var Description = document.createElement("div");
                         var Author = document.createElement("div");
                         const url = `http://localhost:8000/api/${mydata[i].img[0]}/profile_image`
        
                         const options = {
                            method: "GET"
                         }
        
                         let response = await fetch(url, options)
                         const imageBlob = await response.blob()
                         const imageObjectURL = URL.createObjectURL(imageBlob);
    
                         this.todos[i].img[0] = (imageObjectURL)                    
            }
    
    
        } catch (e) {
          console.error(e);
        }
      },
          submitForm(id){
                alert(id)
                alert(this.content)
                axios.post(`http://localhost:8000/api/${id}/comment`, {content:this.content})
                     window.location.reload()
          },
          
          password(){
               const cookieValue = document.cookie.split('; ').find((row) => row.startsWith('jwt='))?.split('=')[1];  
                const headers = {
                  'Content-Type': 'application/json',
                  'Authorization':  `${cookieValue}`
               }     
               axios.post('http://localhost:8000/api/change-password', {content:this.content},{
                   headers: headers
               })
              
          },
    
          onChange(e) {
             const file = e.target.files[0]
             this.item.imageUrl = URL.createObjectURL(file)
             let images = new FormData();
             images.append('image', file); 
             const cookieValue = document.cookie.split('; ').find((row) => row.startsWith('jwt='))?.split('=')[1];       
             axios.post(`http://localhost:8000/api/profile_image`, images,  {
            headers: {Authorization:  `${cookieValue}`}
               })
             window.location.reload()
        }
        },
    
        mounted() {
          this.created();
        
        },
      };
    </script>
    <style scoped>
    h1 {
      text-decoration: underline;
    }
    
    li {
      color: white;
    }
    .card {
      box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2);
      max-width: 300px;
      margin: auto;
      text-align: center;
    }
    
    .title {
      color: grey;
      font-size: 18px;
    }
    
    .buttons {
      border: none;
      outline: 0;
      display: inline-block;
      padding: 8px;
      color: white;
      background-color: #000;
      text-align: center;
      cursor: pointer;
      width: 100%;
      font-size: 18px;
    }
    
    a {
      text-decoration: none;
      font-size: 22px;
      color: black;
    }
    
    .buttons:hover, a:hover {
      opacity: 0.7;
    }
    </style>
    
    
    
    
    <style scoped>
    body{
      width:100% !important;
    }
    .main{
        width: 100% !important;
        display: grid;
        grid-template-columns: repeat(2,1fr);
        margin-left:10% !important;
        margin-top:50px;
    }
    .icons{
      display:flex;
      width:60px !important;
      justify-content:space-between;
      margin-right:0;
      float:right;
    }
    .main div{
      width:50%;
      padding-left:10px;
      font-family: Verdana, Geneva, Tahoma, sans-serif;
    }
    .main img{
        height: 400px;
        width:100%;
        position:relative;
    }
    h1 {
      text-decoration: underline;
    }
    .ed{
      padding-left:10px;
    }
    li {
      color: white;
    }
    .p{
      display:none;
    }
    form{
        display:flex;
        justify-content: space-between;
        width: 350px !important;
        align-items: center;
        height: 40px;
        margin-top:10px;
    }
    form i{
      outline:none;
      border:none !important;
      font-size:20px;
      color:rgb(52, 157, 228);
    }
    form button{
      height:35px;
      width:50px;
    }
    form .input{
      width:300px;
      height:35px;
      outline:none !important;
      padding-left:10px;
      font-family: Verdana, Geneva, Tahoma, sans-serif;
    }
    ::placeholder{
      color:black;
    }
    </style>
    
    
    
    