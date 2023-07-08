<template lang="">
  <div>
    <comment-form></comment-form>
    <comment-list></comment-list>
  </div>
</template>



<script>

import CommentList from './components/CommentList.vue';
import CommentForm from './components/CommentForm.vue';

export default {
  components: {
    CommentList, CommentForm, 
  },
}
</script>



<style >
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  color: aliceblue;
  background-color: rgb(51, 51, 51);
  padding: 30px;
  @media (min-width : 1050px)  {
    width: 1000px;
    margin: 0 auto;
  }
}


</style>