<template lang="">
    <li class="comment">
        <p><strong>Автор  : </strong>{{ comment.author }}</p>
        <p><strong>Комментарий : </strong>{{ comment.text }}</p>
        <p><strong>Время : </strong>{{ comment.createdAt.slice(0, 10) +" "+ comment.createdAt.slice(11, 19)  }}</p>
        <button  class="answer_btn"><strong>Ответить</strong></button>
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



<style scoped>

p {   
    margin-bottom: 10px;
}

.answer_btn {
    margin: 20px ;
    padding: 10px 15px;
    background-color: rgb(27, 27, 27);
    border: 2px solid rgb(149, 149, 149);
    border-radius: 15px;
    color: white;
}
.comment {
    word-break: break-all;
    display:block;
    list-style-type: none;
    padding: 15px;
    border: 3px solid rgb(223, 223, 223);
    border-radius: 20px;
    margin-top: 20px;
    width: 100%;
}  

</style>


