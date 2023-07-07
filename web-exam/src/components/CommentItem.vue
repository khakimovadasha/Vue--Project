<template lang="">
    <li class="comment__item">
        <p>Автор: {{ comment.author }}</p>
        <p class="comment__text">Комментарий: {{ comment.text }}</p>
        <p>Время: {{ comment.createdAt.slice(0, 10) +" "+ comment.createdAt.slice(11, 19)  }}</p>
        <button>Ответить</button>
    </li>
</template>





<script>
export default {
    props: {
        comment: {
            type: Object ,
            required: true ,
        },
        answers: {
            type: Object ,
        }
    }
}
</script>



<style>
p {
    
}

.comment__item {
    word-break: break-all;
    display:block;
    list-style-type: none;
    padding: 15px;
    border: 3px solid rgb(223, 223, 223);
    border-radius: 20px;
    margin-bottom: 20px;
    width: 100%;
}  

</style>


