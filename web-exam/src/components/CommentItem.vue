<template>
  <li class="comment" :class="colorReaction">
    <p class="comment__author"><strong>Автор: </strong>{{ comment.author }}</p>
    <p class="comment__text"><strong>Комментарий: </strong>{{ comment.text }}</p>
    <time class="comment__text" :datetime="formattedDate"><strong>Время: </strong>{{ formattedDate }}</time>
    <p class="comment__reply-count"><strong>Ответов: </strong>{{ getReplyCount(comment.id) }}</p>
    <button class="button button--add" @click="showForm(comment.id)">Добавить комментарий</button>


    <ul v-if="hasReplies(comment.id)" class="comment__replies">
      <comment-item class="comment__child" v-for="reply in getReplies(comment.id)" :comment="reply" :comments="comments" :key="reply.id" :class="colorReaction"></comment-item>
    </ul>
  </li>
</template>

<script>
export default {
  props: {
    comment: {
      type: Object,
      required: true,
    },
    comments: {
      type: Array,
      required: true,
    },
  },
  methods: {
    getReplies(commentId) {
      return this.comments.filter((comment) => comment.parentId === commentId);
    },
    hasReplies(commentId) {
      const replies = this.getReplies(commentId);
      return replies.length > 0;
    },
    getReplyCount(commentId) {
      const replies = this.getReplies(commentId);
      return replies.length || 0;
    },
    showForm(commentId) {
      this.$emit('click', commentId);
    },
  },
  computed: {
    formattedDate() {
      const createdAt = new Date(this.comment.createdAt);
      return createdAt.toLocaleString();
    },
    colorReaction() {
      if (this.comment.reactionSum > 0) {
        return "item_green";
      } else if (this.comment.reactionSum < 0) {
        return "item_red";
      }
      return "";
    },
  },
};
</script>

<style scoped>
.comment {
  word-break: break-all;
  display: block;
  list-style-type: none;
  padding: 15px;
  border: 3px solid rgb(223, 223, 223);
  border-radius: 20px;
  margin-top: 20px;
  width: 100%;
  display: flex;
  flex-direction: column;
  margin-left: 20px;
}

.comment__author,
.comment__text,
.comment__time,
.comment__reply-count {
  margin: 10px 0px;
}

.comment__author strong,
.comment__text strong,
.comment__time strong,
.comment__reply-count strong {
  font-weight: bold;
}

.comment__time {
  font-size: 12px;
  color: #777;
}

.comment__replies {
  margin-top: 10px;
}

.button {
  margin: 20px;
  padding: 10px 15px;
  background-color: rgb(27, 27, 27);
  border: 2px solid rgb(149, 149, 149);
  border-radius: 15px;
  color: white;
  font-size: 16px;
  cursor: pointer;
  transition: background-color 0.3s ease-in-out;
}

.button:hover {
  background: rgb(59, 59, 59);
}

.item_green {
  background: #d7f5db;
}

.item_red {
  background-color: #f8d7d7;
}

.button--add {
  margin-bottom: 0;
}

.comment__child {
  border: 0;
}
</style>
