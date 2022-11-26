<template>
      <div class="main">
        <Navbar />
        <div v-for="(todo, key) in todos" :key="key">
         
            <img :src="`${ todo.img[0]}`" height = '400' width = '400'>
    
    
            <div class="icons">
                <p><i class="fa-regular fa-heart"></i></p>
            
                
            </div>
              <p class='ed'>title: {{ todo.author }}</p><br>
            <p class='ed'>title: {{ todo.title }}</p><br>
            <p class='ed'>description: {{ todo.content }}</p><br>
    
           
    
              
                <h6 id='com' v-for="comments in todo.comments" v-bind:key="comments.id" >coments:{{ comments.content }}</h6>
                <form v-on:submit.prevent="submitForm(todo._id)">
                <input  id="text" placeholder="Add Comment ..."  v-model="content"  name="content" />
                <button id = 'send'><i class="fa-regular fa-paper-plane"></i></button>
                </form>
     
        
    
             
        </div>
        </div>
</template>
    
    
    <script>
      import axios from "axios"; 

      export default {
        data() {
         return {
            todos:  []
         };
        },
       methods: {
            
     async created() {
        try {
            const cookieValue = document.cookie.split('; ').find((row) => row.startsWith('jwt='))?.split('=')[1];                    
            const response = await fetch('http://localhost:8000/api/post', {
              headers: {'Content-Type': 'application/json', 'Authorization': `${cookieValue}`},
              credentials: 'include'
            });
            const content = await response.json();
            const x = JSON.stringify(content)
            const mydata = JSON.parse(x);  
            this.todos = mydata
     
      
            for(let i = 0;i < this.todos.length; i++){
                         const url = `http://localhost:8000/api/${mydata[i].img[0]}/post_image`
        
                         const options = {
                            method: "GET"
                         }
        
                         const response = await fetch(url, options)
                         const imageBlob = await response.blob()
                         const imageObjectURL = URL.createObjectURL(imageBlob);
    
                         this.todos[i].img[0] = (imageObjectURL)
                         
            }
        } catch (e) {
          console.error(e);
        }
      },
        submitForm(id){
                axios.post(`http://localhost:8000/api/${id}/comment`, {content:this.content})
                     window.location.reload()
            },
        },
    
        mounted() {
          this.created();
        
        },
      };
    </script>
    <style scoped>
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
    form input{
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
    
    
    
    