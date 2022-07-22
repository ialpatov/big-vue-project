<template>
    <form @submit.prevent>
        <h4>Создание поста</h4>
        <my-input placeholder="Название" v-model="post.title" v-focus/>
        <my-input placeholder="Описание" v-model="post.body"/>
        <my-button class="btn_flex" @click="createPost">Создать</my-button>
    </form>
</template>

<script>
import MyInput from "@/components/UI/MyInput";
export default {
    components: {MyInput},
    data() {
        return {
            post: {
                title: '',
                body: ''
            }
        }
    },
    methods: {
        createPost() {
            if(this.post.title.trim() && this.post.body.trim()) {
                this.$emit('create', this.post)
                this.post = {
                    title: '',
                    body: ''
                }
            }
        }
    },
}
</script>

<style scoped>
    form {
        display: flex;
        flex-direction: column;
    }
    
    .btn_flex {
        align-self: flex-start;
        margin-top: 15px;
    }
    
</style>