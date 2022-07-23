<template>
    <div>
        <h1>Страница с постами</h1>
        <my-input :model-value="searchQuery" @update:model-value="setSearchQuery" placeholder="Поиск..." v-focus></my-input>
        <div class="app__btns mt-15">
            <my-button @click="showDialogue">Создать пост</my-button>
            <my-select :model-value="selectedSort" @update:model-value="setSelectedSort" :options="sortOptions"></my-select>
        </div>
        
<!--        <my-dialogue v-model:show="dialogueVisible">
            <post-form @create="createPost"></post-form>
        </my-dialogue>-->
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
import {mapState, mapActions, mapGetters, mapMutations} from 'vuex'
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
            dialogueVisible: false,
        }
    },
    methods: {
        ...mapMutations({
            setPage: 'post/setPage',
            setSearchQuery: 'post/setSearchQuery',
            setSelectedSort: 'post/setSelectedSort'
        }),
        ...mapActions({
            loadMorePosts: 'post/loadMorePosts',
            fetchPosts: 'post/fetchPosts'
        }),
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
    },
    mounted() {
        this.fetchPosts()
    },
    watch: {
    
    },
    computed: {
        ...mapState({
            posts: state => state.post.posts,
            isPostsLoading: state => state.post.isPostsLoading,
            selectedSort: state => state.post.selectedSort,
            searchQuery: state => state.post.searchQuery,
            page: state => state.post.page,
            limit: state => state.post.limit,
            totalPages: state => state.post.totalPages,
            sortOptions: state => state.post.sortOptions
        }),
        ...mapGetters({
            sortedPosts: 'post/sortedPosts',
            sortedAndSearchedPosts: 'post/sortedAndSearchedPosts'
        })
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