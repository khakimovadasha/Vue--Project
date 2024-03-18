<template>
  <div>
    <CommentForm ref="commentForm" />

    <button class="button button--server" @click="toggleConnection">{{ isConnected ? 'Отключиться от сервера' : 'Подключиться к серверу' }}</button>
    <h1 class="title">Комментарии:</h1>
    <p class="comment-count">Всего комментариев: {{ comments.length }}</p>
    <comment-item :comment="comment" :comments="comments" v-for="comment in reversedComments" :key="comment.id" @click="showCommentForm"></comment-item>

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
    showCommentForm(commentId) {
      this.$refs.commentForm.newComment.parentId = commentId;
      this.$refs.commentForm.showForm();
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
  computed: {
  reversedComments() {
    if (this.isConnected) {
      return this.comments.reverse();
    } else {
      return this.comments;
    }
  }
}
};
</script>

<style scoped>
.button {
  margin: 20px;
  padding: 10px 15px;
  background-color: white;
  border: 2px solid grey;
  border-radius: 15px;
  font-size: 16px;
  cursor: pointer;
  transition: background-color 0.3s ease-in-out;
}

.button:hover {
  background-color: lightgrey;
}

.button--server {
  margin-bottom: 0;
}

.title {
  margin-top: 15px;
  font-size: 24px;
}

.comment-count {
  margin-top: 15px;
  font-size: 20px;
}
</style>
