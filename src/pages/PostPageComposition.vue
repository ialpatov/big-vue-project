<template>
    <div>
        <h1>Страница с постами</h1>
        
        
        <my-input v-model="searchQuery" placeholder="Поиск..." v-focus></my-input>
        <div class="app__btns mt-15">
            <my-button>Создать пост</my-button>
            <my-select v-model="selectedSort" :options="sortOptions"></my-select>
        </div>
        
        <my-dialogue v-model:show="dialogueVisible">
            <post-form ></post-form>
        </my-dialogue>
        <post-list :posts="sortedAndSearchedPosts" v-if="!isPostsLoading"></post-list>
        <div v-else class="mt-15">Идет загрузка...</div>
        <div v-intersection="loadMorePosts" class="observer"></div>
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
import {ref} from "vue";
import {usePosts} from "@/hooks/usePosts";
import useSortedPosts from "@/hooks/useSortedPosts";
import useSortedAndSearchedPosts from "@/hooks/useSortedAndSearchedPosts";
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
            sortOptions: [
                {value: 'title', name: 'По названию'},
                {value: 'body', name: 'По описанию'}
            ]
        }
    },
    setup(props) {
        const {posts, isPostsLoading, totalPages} = usePosts(10)
        const {sortedPosts, selectedSort} = useSortedPosts(posts)
        const {searchQuery, sortedAndSearchedPosts} = useSortedAndSearchedPosts(sortedPosts)
        
        return {
            searchQuery,
            sortedAndSearchedPosts,
            isPostsLoading,
            totalPages,
            selectedSort
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