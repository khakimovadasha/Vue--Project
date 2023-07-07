<template>
    <div>
        <button @click="showForm">Добавить комментарий</button>
        <div v-if="showCommentForm" class="comment-form-overlay">
            <div class="comment-form">
                <h2>Добавить комментарий</h2>
                <form @submit.prevent="addComment">
                    <label for="author">Автор:</label>
                    <input type="text" id="author" v-model="newComment.author" placeholder="Коммент" required><br>
                    <label for="text">Комментарий:</label>
                    <textarea id="text" v-model="newComment.text" required></textarea><br>
                <button type="submit">Добавить</button>
                </form>
            </div>
        </div>
        <div v-if="comments.length > 0">
            <ul>
                <li v-for="comment in comments" :key="comment.id">
                    <strong>{{ comment.author }}</strong>: {{ comment.text }}
                </li>
            </ul>
        </div>
    </div>
</template>


<script>
    export default {
        data() {
            return {
                showCommentForm: false,
                newComment: {
                    author: '',
                    text: ''
                },
        comments: []
        };
    },
    methods: {
        showForm() {
            this.showCommentForm = true;
        },
        async addComment() {
            console.log("It's addComment")
            const comment = {
                author: this.newComment.author,
                text: this.newComment.text
            };
        try {
            const response = await fetch ('http://194.67.93.117/comments', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json',
                    'Username': 'khakimovadasha'
                },
                body: JSON.stringify(comment)
            });

            if (response.ok) {
                const savedComment = await response.json();
                this.comments.push(savedComment);
                console.log('Комментарий успешно добавлен на сервер.');
            } 
            else {
                console.log('Ошибка при добавлении комментария на сервер.');
            }
        } 
        catch (error) {
            console.log('Произошла ошибка:', error);
        }
        this.newComment.author = '';
        this.newComment.text = '';
        this.showCommentForm = false;
        }
    }
};
</script>



<style>
.comment-form-overlay {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.5);
    display: flex;
    align-items: center;
    justify-content: center;
}

.comment-form {
    background-color: rgb(70, 67, 67);
    padding: 20px;
    border-radius: 10px;
    border: solid 2px rgb(211, 211, 211);
}
</style>