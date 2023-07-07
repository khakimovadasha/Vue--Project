
<template>
    <div>
        <button class="add_btn" @click="showForm">Добавить комментарий</button>
        <div v-if="showCommentForm" class="form">
            <div class="form_comment">
                <h2>Добавить комментарий</h2>
                <form @submit.prevent="addComment">
                    <label for="author">Автор:</label>
                    <input class="form_comment__input" type="text" id="author" v-model="newComment.author" placeholder="Коммент" required><br>
                    <label for="text">Комментарий:</label>
                    <textarea class="form_comment__textarea" id="text" v-model="newComment.text" required></textarea><br>
                    <label>Реакция:</label>
                    <div>
                        <svg @click="setReaction(0)" :class="{ active: newComment.reaction === 0 }"
                        xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 30 30">
                            <g stroke="#bffe81" stroke-linecap="round" stroke-width="1.5">
                            <path stroke-linejoin="round" d="M9 14c.2.5.5.9.9 1.2 1.2 1 3 1 4.2 0 .4-.3.7-.7.9-1.2"/>
                            <path stroke-linejoin="round" d="M19 12a7 7 0 1 1-14 0 7 7 0 0 1 14 0Z" clip-rule="evenodd"/>
                            <path d="M9 11v-1M15 11v-1"/>
                            </g>
                        </svg>

                        <svg @click="setReaction(1)" :class="{ active: newComment.reaction === 1 }"
                        xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 30 30">
                            <path fill="#fff" d="M9.8 10a.8.8 0 0 0-1.6 0h1.6Zm-1.6 1a.8.8 0 0 0 1.6 0H8.2Zm7.6-1a.8.8 0 0 0-1.6 0h1.6Zm-1.6 1a.8.8 0 0 0 1.6 0h-1.6ZM9 14.3a.8.8 0 0 0 0 1.4v-1.4Zm6 1.4a.8.8 0 0 0 0-1.4v1.4Zm2-8.6-.6.5.5-.5ZM8.1 10v1h1.6v-1H8.2Zm6 0v1h1.6v-1h-1.6ZM9 15.8h6v-1.6H9v1.6Zm9.3-3.8c0 3.5-2.8 6.3-6.3 6.3v1.4c4.3 0 7.8-3.4 7.8-7.7h-1.6ZM12 18.3A6.2 6.2 0 0 1 5.7 12H4.3c0 4.3 3.5 7.8 7.8 7.8v-1.6ZM5.7 12c0-3.5 2.8-6.3 6.3-6.3V4.3A7.7 7.7 0 0 0 4.2 12h1.5ZM12 5.7c1.7 0 3.2.7 4.4 1.9l1-1A7.8 7.8 0 0 0 12 4.2v1.5Zm4.4 1.9a6.3 6.3 0 0 1 1.9 4.4h1.4c0-2-.8-4-2.2-5.5l-1 1Z"/>
                        </svg>
                        
                        <svg @click="setReaction(-1)" :class="{ active: newComment.reaction === -1 }"
                            xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 30 30">
                            <g stroke="#ff4242" stroke-linecap="round" stroke-width="1.5">
                            <path stroke-linejoin="round" d="M15 15a3 3 0 0 0-.9-1.2c-1.2-1-3-1-4.2 0A3 3 0 0 0 9 15"/>
                            <path d="M9 10V9"/>
                            <path stroke-linejoin="round" d="M19 12a7 7 0 1 1-14 0 7 7 0 0 1 14 0Z" clip-rule="evenodd"/>
                            <path d="M15 10V9"/>
                            </g>
                        </svg>
                        
                    </div><br>
                    <button type="submit">Добавить</button>
                </form>
            </div>
        </div>
        <div v-if="comments.length > 0">
            <ul>
                <li v-for="comment in comments" :key="comment.id">
                    <strong>{{ comment.author }}</strong>: {{ comment.text }} (Реакция: {{ comment.reaction }})
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
                text: '',
                reaction: 0
            },
            comments: []
        };
    },
    methods: {
        showForm() {
            this.showCommentForm = true;
            },
        setReaction(reaction) {
            this.newComment.reaction = reaction;
            },
        async addComment() {
            const comment = {
                author: this.newComment.author,
                text: this.newComment.text,
                reaction: this.newComment.reaction
                };
        try {
            const response = await fetch('http://194.67.93.117/comments', {
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
            this.newComment.reaction = 0;
            this.showCommentForm = false;
            }
        }
    };
</script>






<style scoped>
.form {
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

.add_btn {
    margin: 20px ;
    padding: 10px 15px;
    background-color: white;
    border: 2px solid grey;
    border-radius: 15px;
}

svg {
    width:50px; 
}
svg.active {
    width: 70px;
}


.form_comment {
    background-color: rgb(70, 67, 67);
    padding: 20px;
    border-radius: 10px;
    border: solid 2px rgb(211, 211, 211);
}
</style>

