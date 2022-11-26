<template>
    <div>
      <form action='http://localhost:8000/api/post_image' enctype="multipart/form-data" method="post" target="hiddenFrame">
      <div class="form-group">
        <input type="file" class="form-control-file" name="image">
        <input type="text" class="form-control" placeholder="Post Title" name="title">
        <input type="text" class="form-control" placeholder="Post Description" name="content">
        <input type="submit" id="add" value="Add Post" class="btn btn-default">            
      </div>
    </form>
    </div>
</template>
    
    
    <script>
    import axios from "axios";
    export default {
      data() {
          return {
             FILE: null,
             name: ''
          };
        },
        methods: {
            onFileUpload (event) {
              this.FILE = event.target.files[0]
            },
            onSubmit() {
              const formData = new FormData()
              formData.append('avatar', this.FILE, this.FILE.name)
              formData.append('name', this.name)
              axios.post('http://localhost:8000/api/post', formData, {
              }).then((res) => {
                console.log(res)
              })
              
            }  
        }
    }
    </script>
    <style scoped>
    body{
      width:100% !important;
    }
    .form-group{
        width: 100% !important;
        display: flex;
        align-items:center;
        flex-direction:column;
        margin-top:50px;
    }
    .form-group input{
      width:300px !important;
      height:35px;
      outline:none !important;
      padding-left:10px;
      font-family: Verdana, Geneva, Tahoma, sans-serif;
    }
    </style>
    