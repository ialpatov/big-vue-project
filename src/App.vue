<template>
    <div class="app">
        <form action="#">
            <h4>Создание поста</h4>
            <input class="input" type="text" placeholder="Название" v-model="title">
            <input class="input" type="text" placeholder="Описание" v-model="body">
            <button class="btn" @click="createPost">Создать</button>
        </form>
        <div class="post" v-for="post in posts">
            <div><strong>Название:</strong> {{ post.title }}</div>
            <div><strong>Описание:</strong> {{ post.body }}</div>
        </div>
    </div>

    
</template>

<script>
    export default {
        data() {
          return {
            posts: [
                {id: 1, title: 'Javascript', body: 'Описание поста'},
                {id: 2, title: 'Javascript 2', body: 'Описание поста 2'},
                {id: 3, title: 'Javascript 3', body: 'Описание поста 3'}
            ],
            title: '',
            body: '',
          }
        },
       methods: {
            createPost() {
                this.posts.push({id: this.posts[this.posts.length - 1].id, title: this.title, body: this.body})
                this.body = ''
                this.title = ''
            }
        }

    }

</script>

<style>
    * {
        margin: 0;
        padding: 0;
        box-sizing: border-box;
    }
    
    .app {
        padding: 20px;
    }
    
    .post {
        padding: 15px;
        border: 2px solid teal;
        margin-top: 15px;
    }
    
    form {
        display: flex;
        flex-direction: column;
    }
    
    .btn {
        align-self: flex-end;
        margin-top: 15px;
        padding: 10px 15px;
        background: none;
        color: teal;
        border: 1px solid teal;
        cursor: pointer;
    }
    
    .input {
        width: 100%;
        border: 1px solid teal;
        padding: 10px 15px;
        margin-top: 15px;
    }
</style>