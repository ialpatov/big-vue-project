<template>
    <div>
        <h1>Страница с постами</h1>
        <my-input v-model="searchQuery" placeholder="Поиск..." v-focus></my-input>
        <div class="app__btns mt-15">
            <my-button @click="showDialogue">Создать пост</my-button>
            <my-select v-model="selectedSort" :options="sortOptions"></my-select>
        </div>
        
        <my-dialogue v-model:show="dialogueVisible">
            <post-form @create="createPost"></post-form>
        </my-dialogue>
        <post-list :posts="sortedAndSearchedPosts" @remove="removePost" v-if="!isPostsLoading"></post-list>
        <div v-else class="mt-15">Идет загрузка...</div>
        <div v-intersection="loadMorePosts" class="observer"></div>
        <!--        <div class="page__wrapper mt-15">
                    <div v-for="pageNum in totalPages" :key="pageNum" class="page" :class="{
                        'current_page': pageNum === page
                    }" @click="changePage(pageNum)">
                        {{ pageNum }}
                    </div>
                </div>-->
    </div>

</template>

<script>
import PostForm from "@/components/PostForm";
import PostList from "@/components/PostList";
import MyDialogue from "@/components/UI/MyDialogue";
import MyButton from "@/components/UI/MyButton";
import axios from 'axios'
import MySelect from "@/components/UI/MySelect";
import MyInput from "@/components/UI/MyInput";
export default {
    components: {
        MyInput,
        MySelect,
        MyButton,
        MyDialogue,
        PostList, PostForm
    },
    data() {
        return {
            posts: [],
            dialogueVisible: false,
            isPostsLoading: false,
            selectedSort: '',
            searchQuery: '',
            page: 1,
            limit: 10,
            totalPages: 0,
            sortOptions: [
                {value: 'title', name: 'По названию'},
                {value: 'body', name: 'По описанию'}
            ]
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
                const response = await axios.get('https://jsonplaceholder.typicode.com/posts', {
                    params:  {
                        _page: this.page,
                        _limit: this.limit
                    }
                })
                this.totalPages = Math.ceil(response.headers['x-total-count'] / this.limit)
                this.posts = response.data
            } catch (e) {
                alert('Ошибка!')
            } finally {
                this.isPostsLoading = false
            }
        },
        async loadMorePosts() {
            try {
                this.page += 1
                const response = await axios.get('https://jsonplaceholder.typicode.com/posts', {
                    params:  {
                        _page: this.page,
                        _limit: this.limit
                    }
                })
                this.totalPages = Math.ceil(response.headers['x-total-count'] / this.limit)
                this.posts = [...this.posts, ...response.data]
            } catch (e) {
                alert('Ошибка!')
            } finally {
            }
        },
        /*changePage(pageNum) {
            this.page = pageNum
        }*/
    },
    mounted() {
        this.fetchPosts()
        /*const options = {
            rootMargin: '0px',
            threshold: 1.0
        }
        const callback = (entries, observer) => {
            if(entries[0].isIntersecting && this.page < this.totalPages) {
                this.loadMorePosts()
            }
        };
        const observer = new IntersectionObserver(callback, options);
        observer.observe(this.$refs.observer)*/
    },
    watch: {
        /* selectedSort(newValue) {
             this.posts.sort((post1, post2) => post1[newValue].localeCompare(post2[newValue]))
         }*/
        /*page() {
            this.fetchPosts()
        }*/
    },
    computed: {
        sortedPosts() {
            return [...this.posts].sort((post1, post2) => post1[this.selectedSort]?.localeCompare(post2[this.selectedSort]))
        },
        sortedAndSearchedPosts() {
            return this.sortedPosts.filter(post => post.title.toLowerCase().includes(this.searchQuery.toLowerCase()))
        }
    }
    
}

</script>

<style>


.app__btns {
    display: flex;
    justify-content: space-between;
    
}

.mt-15 {
    margin-top: 15px;
}

/*.page__wrapper{
    display: flex;
    justify-content: center;
}

.page {
    border: 1px solid teal;
    padding: 10px;
}

.page:hover {
    background: antiquewhite;
    cursor: pointer;
}

.current_page {
    border: 3px solid teal;
    background: antiquewhite;
}*/

.observer {
    height: 30px;
    background: green;
}

</style>