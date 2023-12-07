<template>
  <div class="app" @click = "show = false">
    <header>
        <h1>FIRST VUE BLOG</h1>
        <transition name = "fade">
            <div v-if = "show" class="alert">Post must contain title, text and theme</div>
        </transition>
    </header>
    <form v-on:submit.prevent="onSubmit" id = "create_post">
        <h3>Create post</h3>
        <input v-bind:value = "title" @input = "inputPostTitle" class = "input_title" type="text" maxlength="50" placeholder = "Print title here...">
        <input v-bind:value = "theme" @input = "inputPostTheme" type="text" maxlength="20" class = "input_theme" placeholder = "Print theme here...">
        <textarea v-bind:value = "body" @input = "inputPostBody" class = "input_body" id="input_body" maxlength="500" cols="30" rows="10" placeholder="Print text here..."></textarea>
        <button @click = "createPost" v-on:click.stop="doThis">Submit</button>
    </form>
    <div class="welcome_message" v-if = "showMessage">
        There are no posts here yet
    </div>
    <div class="post" v-for = "post in posts">
        <h3 class="title">{{ post.title }}</h3>
        <div class = "info">
            <span class="theme">{{ post.theme }}</span> | <span class="date">{{ post.date }}</span>
        </div>
        <div class = "body">{{ post.body }}</div>
    </div>
  </div>
</template>

<script>
  import { nanoid } from 'nanoid'
  export default {
    data(){
        return {
            posts: [],
            id:nanoid(),
            title:'',
            body:'',
            theme:'',
            date:'',
            show:false,
            showMessage:true
        }
    },
    methods: {
        createPost(){
            function getThisDate(){
                let date = new Date();
                let months = [
                    'Jan',
                    'Feb',
                    'Mar',
                    'Apr',
                    'May',
                    'Jun',
                    'Jul',
                    'Aug',
                    'Sep',
                    'Oct',
                    'Nov',
                    'Dec'
                ]
                let month = months[date.getMonth()];
                return date.getDate() + ' ' + month + ' ' + date.getFullYear();
            }
            
            const newPost = {
                id:this.id,
                title: this.title,
                theme: this.theme,
                body: this.body,
                date:getThisDate()
            }

            if(this.theme == "" || this.title == "" || this.body == ""){
                this.show = !this.show;
            }
            else{
                this.posts.unshift(newPost);
                this.title = '';
                this.theme = '';
                this.date = '';
                this.body = '';
                this.id = '';
                this.showMessage = false;
            }

            
        },
        inputPostTitle(event){
            this.title =  event.target.value;
        },
        inputPostTheme(event){
            this.theme = event.target.value;
        },
        inputPostBody(event){
            this.body = event.target.value;
        },
    }
  }
</script>

<style>
    @font-face {
        font-family: 'Roboto bold';
        src: url('../public/fonts/Roboto/Roboto-Bold.ttf');
    }

    @font-face {
        font-family: 'Roboto regular';
        src: url('../public/fonts/Roboto/Roboto-Regular.ttf');
    }

    @font-face {
        font-family: 'Roboto medium';
        src: url('../public/fonts/Roboto/Roboto-Medium.ttf');
    }

    * {
      margin: 0;
      outline: 0;
      box-sizing: border-box;
    }
    
    body {
     -webkit-font-smoothing: antialiased;
     padding-bottom: 10px;
     min-width: 600px;
    }
    
    
    button {
     cursor: pointer;
     height: 30px;
     font-size: 18px;
    }

    *::-webkit-scrollbar{
       background-color:transparent;
    }

    *::-webkit-scrollbar-thumb{
        border-radius: 10px;
        background-color: grey;
        border: 5px solid transparent;
        background-clip: content-box;
    }

    *::selection{
        background-color:rgb(245, 49, 49);
        color:white;
    }

    header{
        display: flex;
        justify-content: space-around;
        align-items: center;
        width: 80vw;
        height: 100px;
        margin-bottom:15px;
       border-bottom: solid rgb(245, 49, 49) 1px;
    }

    h1, h2, h3, h4{
        font-family: 'Roboto bold';
    }

    .app{
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
    }

    form{
        display:flex;
        flex-direction:column;
        border-bottom: 1px solid #cfcdcd;
        padding-bottom:15px;
    }

    .post{
        width: 600px;
        margin-top:15px;
        border-bottom: 1px solid #cfcdcd;
        padding-bottom:15px;
    }

    .post > *{
        text-indent: 10px;
        margin-top: 5px;
        word-wrap: break-word;
    }

    .title{
        font-family: 'Roboto bold';
    }

    .theme{
        font-weight:500;
        font-family: 'Roboto medium';
    }

    .date{
        font-weight: 500;
        font-family: 'Roboto medium';
    }

    .body{
        font-family:'Roboto regular';
    }

    input{
        margin-top: 15px;
        border-radius:10px;
        width: 500px;
        height: 50px;
        border:1px solid grey;
        font-size:18px;
        font-family: 'Roboto regular';
        padding: 15px;
    }

    .input_body{
        margin-top: 15px;
        border-radius:10px;
        width: 500px;
        height: 100px;
        border:1px solid grey;
        font-size:18px;
        font-family: 'Roboto regular';
        padding: 15px;
        resize: none;
        overflow-y:scroll;
    }

    .input_body:focus{
        outline:rgb(245, 49, 49);
        border-color: rgb(245, 49, 49);
    }

    .input_body:focus::-webkit-input-placeholder 
    {
        color: transparent;
    }

    button{
        font-family: 'Roboto regular';
        width: 100px;
        align-self:flex-end;
        margin-top:15px;
        border-radius: 5px;
        border: 1px solid grey;
        background:none;
        cursor:pointer;
    }

    .welcome_message{
        color:grey;
        font-family: 'Roboto regular';
    }

    button:hover{
        background-color: rgb(245, 49, 49);
        border-color:rgb(245, 49, 49);
        color:white;
    }

    input:focus{
        outline:rgb(245, 49, 49);
        border-color: rgb(245, 49, 49);
    }

    input:focus::-webkit-input-placeholder 
    {
        color: transparent;
    }

    .alert{
        position:fixed;
        background-color: rgb(245, 49, 49);
        color:white;
        font-family: 'Roboto medium';
        padding: 25px;
        border-radius: 10px;
        font-size: 18px;
    }

    .fade-enter-active,
    .fade-leave-active {
        transition: opacity 0.5s ease;
    }

    .fade-enter-from,
    .fade-leave-to {
      opacity: 0;
    }

    @media screen and (max-width:1000px){

        h1{
            font-size: 48px;
        }

        h3{
            font-size: 36px;
        }

        header{
            border-width: 3px;
        }

        .post{
            width: 90vw;
            border-width: 3px;
            font-size: larger;
        }

        button{
            align-self: center;
            background-color: rgb(245, 49, 49);
            color:white;
            width: 40vw;
            height: 100px;
            font-size:36px;
            border-radius: 25px;
            margin-top: 50px;
            border:none;
        }

        input{
            font-size: 36px;
            width: 90vw;
            height: 100px;
            border-width: 3px;
        }

        .input_body{
            font-size:36px;
            width: 90vw;
            height: 300px;
            border-width: 3px;
        }

        .input_body::-webkit-input-placeholder 
        {
            font-size:36px
        }
        
        .alert{
            font-size: 36px;
        }
    }
</style>