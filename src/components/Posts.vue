<script setup>
import { defineProps, ref, computed } from 'vue';
import Comments from './Comments.vue'
import moment from 'moment';

const props         = defineProps({
  posts:{
    type: Array,
    required: true,
  }
});

// define emits
const emit = defineEmits({
  increaseLikeCount: 'null',
  deletePost: 'null',
})

//posts
const posts = ref(props.posts);

// reverse post 
const reversedPost = computed(()=>{
  return [...posts.value].reverse()
});

// assign empty array to store comment for each post.
const newComment      = ref([]);

// create comment
function createComment(index){
  // if comment exists, then push into comment array
  if(newComment.value[index]){
      posts.value[index].comments.push({
      id            : posts.value[index].comments.length + 1, // id incremented by 1 with post's last comment id, if there is no comment then id is initial 1
      text          : newComment.value[index],
      date          : moment().format('YYYY-MM-DD HH:mm:ss')
    })
  }
  newComment.value[index] = '';  //empty comment
}

// delete comment
function deleteComment(post,commentIndex){
  post.comments.splice(commentIndex,1)
}

</script>
<template>
<!-- post list start -->
<div class="posts mt-4">
    <div class="container">
      <div class="nav nav-tabs mb-4">
        <h3>Posts</h3>
      </div>

      <div class="row">
        <!-- post v-for loop here -->
        <div class="col-md-4" v-for="(post,index) in reversedPost" :key="post.id">
          <div class="card mb-4">
            <div class="card-body">
              <h5 class="card-title">{{ post.title }}</h5>
              <h6 class="card-subtitle mb-2 text-body-secondary">{{ moment(post.date).fromNow() }}</h6>
              <p class="card-text">{{ post.content }}</p>
              <p class="card-text">
                <small v-if="post.likes > 1">{{ post.likes }} Likes, </small>
                <small v-else-if="post.likes == 1">{{ post.likes }} Like, </small>
                <small v-else>No Likes, </small>
                <small :class="post.comments.length>=3?'text-success': ''" @click="post.commentVisibility=!post.commentVisibility" style="cursor: pointer;">{{ 
                  post.comments.length==1 ? post.comments.length + ' Comment':
                  post.comments.length>1 ? post.comments.length + ' Comments': 'No Comments'
                }}</small>
              </p>

              <div class="comments mb-3">
                <div class="comments-input d-flex mb-3">
                  <input v-model="newComment[reversedPost.length - (index+1)]" @keydown.enter="createComment(reversedPost.length - (index+1))" type="text" class="form-control form-control-sm me-2" placeholder="Write Comment">
                  <button @click="createComment(reversedPost.length - (index+1))" class="btn btn-sm btn-success"><i class="bi bi-send"></i></button>
                </div>
                <Comments :post="post" @deleteComment="deleteComment"></Comments>
              </div>
              <button class="btn btn-sm btn-primary" @click="emit('increaseLikeCount',(reversedPost.length - (index+1)))">Like</button> 
              <button class="btn btn-sm btn-danger float-end" @click="emit('deletePost',(reversedPost.length - (index+1)))"><i class="bi bi-trash"></i></button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
  <!-- post list end -->
</template>