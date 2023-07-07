<template>
    <div>
        <button class="server_btn" @click="toggleConnection">{{ isConnected ? 'Отключиться от сервера' : 'Подключиться к серверу' }}</button>
        <h1>Комментарии:</h1>
        <comment-item :comment="comment" v-for="comment in comments" :key="comment.id"></comment-item>
    </div>
</template>

<script>
import CommentItem from './CommentItem.vue';

export default {
    components: {
        CommentItem
    },
    methods: {
        async fetchComments() {
            let response = await fetch('http://194.67.93.117/comments');
            this.comments = await response.json();
        },
        async toggleConnection() {
            if (this.isConnected) {
                this.isConnected = false;
            } 
            else {
                this.isConnected = true;
                await this.fetchComments();
            }
        },
    },
    created() {
        this.fetchComments();
    },
    data() {
        return {
            comments: [],
            isConnected: true,
            };
        },
    };
</script>


<style>
.server_btn {
    margin: 20px ;
    padding: 10px 15px;
    background-color: white;
    border: 2px solid grey;
    border-radius: 15px;
}
</style>
