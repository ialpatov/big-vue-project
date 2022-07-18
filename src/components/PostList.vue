<template>
    <div class="post-list">
        <h3>Список пользователей</h3>
        <post-item v-for="post in posts" :post="post" :key="post.id" @remove="$emit('remove', post)"></post-item>
    </div>
</template>

<script>
import PostItem from "@/components/PostItem";
export default {
    components: {PostItem},
    props: {
        posts: {
            required: true,
            type: Array
        }
    }

}
</script>

<style scoped>
    .post-list {
        margin-top: 15px;
    }
</style>