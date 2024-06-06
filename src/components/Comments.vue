<script setup>
import { computed, defineProps, ref} from 'vue';
import moment from 'moment';
const props         = defineProps({
    post:{
    type: Object,
    required: true,
  }
});

// define emits
const emit = defineEmits({
  deleteComment: 'null',
})
 
const allComments = ref(props.post.comments)
// reverse post 
const reversedComment = computed(()=>{
  return [...allComments.value].reverse()
});

</script>  

<template>
    <!-- comments section start -->
    <!-- {{ reversedComment }} -->
        <!-- comment v-for loop here -->
        <div v-if="post.commentVisibility" class="comment mb-3 ms-3" v-for="(comment,commentIndex) in reversedComment">
            <h6 class="card-title small">John Doe <span style="cursor: pointer;" @click="emit('deleteComment', post, reversedComment.length - (commentIndex+1))" class="text-danger float-end cursor-pointer">X</span></h6>
            <p class="card-subtitle mb-1 text-body-secondary small">{{ moment(comment.date).fromNow() }}</p>
            <p class="card-text small">{{ comment.text }}</p>
        </div>
        <hr class="my-2 very-low-opacity">
            
    <!-- comments section end -->
</template>