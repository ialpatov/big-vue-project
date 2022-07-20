<template>
    <div class="post-list" v-if="posts.length">
        <h3>Список постов</h3>
        <transition-group name="post-list">
            <post-item v-for="post in posts" :post="post" :key="post.id" @remove="$emit('remove', post)"></post-item>
        </transition-group>
    </div>
    <h2 v-else style="color: darkred" class="post-list">
        Постов нет
    </h2>
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
    .post-list-enter-active,
    .post-list-leave-active {
        transition: all 0.4s ease;
    }
    .post-list-enter-from,
    .post-list-leave-to {
        opacity: 0;
        transform: translateX(130px);
    }
    .post-list-move {
        transition: all 0.4s ease;
    }

</style>