<template>
    <div class="app">
        <h1>Страница с постами</h1>
        <my-button @click="showDialogue" class="mt-15">Создать пост</my-button>
        <my-dialogue v-model:show="dialogueVisible">
            <post-form @create="createPost"></post-form>
        </my-dialogue>
        <post-list :posts="posts" @remove="removePost" v-if="!isPostsLoading"></post-list>
        <div v-else class="mt-15">Идет загрузка...</div>
    </div>

</template>

<script>
    import PostForm from "@/components/PostForm";
    import PostList from "@/components/PostList";
    import MyDialogue from "@/components/UI/MyDialogue";
    import MyButton from "@/components/UI/MyButton";
    import axios from 'axios'
    export default {
        components: {
            MyButton,
            MyDialogue,
            PostList, PostForm
        },
        data() {
          return {
            posts: [],
            dialogueVisible: false,
            isPostsLoading: false
          }
        },
       methods: {
            createPost(post) {
                this.posts.push({id: Date.now(), title: post.title, body: post.body})
                this.dialogueVisible = false
            },
            removePost(post) {
                this.posts = this.posts.filter(p => p.id !== post.id)
            },
            showDialogue() {
                this.dialogueVisible = true
            },
            async fetchPosts() {
                try {
                    this.isPostsLoading = true
                    const response = await axios.get('https://jsonplaceholder.typicode.com/posts?_limit=10')
                    this.posts = response.data
                } catch (e) {
                    alert('Ошибка!')
                } finally {
                    this.isPostsLoading = false
                }
            }
        },
        mounted() {
            this.fetchPosts()
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
    
    .mt-15 {
        margin-top: 15px;
    }
   
</style>