<script setup>
import { computed, ref, watch } from 'vue';
import moment from 'moment';
import Posts from './components/Posts.vue'
const formData = ref({
  title                : '',
  content              : ''
});

//initial some posts start
const posts = ref([
  {
    id                : 1,
    title             : 'Post 1',
    content           : 'This is a wider card with supporting text below as a natural lead-in to additional content. This content is a little bit longer.',
    likes             : 0,
    comments          : [],
    date              : '2024-05-24 11:00:00',
    commentVisibility : false
  },
  {
    id                : 2,
    title             : 'Post 2',
    content           : 'This is a wider card with supporting text below as a natural lead-in to additional content. This content is a little bit longer.',
    likes             : 0,
    comments          : [],
    date              : '2024-05-24 11:00:00',
    commentVisibility : false
  },
  {
    id                : 3,
    title             : 'Post 3',
    content           : 'This is a wider card with supporting text below as a natural lead-in to additional content. This content is a little bit longer.',
    likes             : 0,
    comments          : [],
    date              : '2024-05-24 11:00:00',
    commentVisibility : false
  }
]);
//initial some posts end


// increase like for individual post
function increaseLikeCount(index){
  posts.value[index].likes++;
}

// create post
function createPost(){
  posts.value.push({
    id                : posts.value.length?posts.value[(posts.value.length)-1].id + 1 : 1, // id incremented by 1 with last post id, if there is no post then id is initial 1
    title             : formData.value.title,
    content           : formData.value.content,
    likes             : 0,
    comments          : [],
    date              : moment().format('YYYY-MM-DD HH:mm:ss'),
    commentVisibility : false
  })

  formData.value.title    = ''; //empty form title
  formData.value.content  = ''; //empty form content
}

//post create confirmation
watch(
  ()=>posts.value.length,
  (newValue,oldValue)=>{
    if(newValue>oldValue){
      alert('A new post has been created.');
    }
  },
  {immediate: true}
)

// 10 likes confirmation

watch(
  ()=>{
    posts.value.forEach(post=>{
      if(post.likes == 10 && !post.likeConfirmation){
        post.likeConfirmation = true;
        alert("A post has reached 10 likes.");
      }
    })
  }
)

// comments congratualtions
watch(
  ()=>{
    posts.value.forEach(post=>{
      if(post.comments.length == 3 && !post.CommentsConfirmation){
        post.CommentsConfirmation = true;
        alert("A post has reached 3 comments.");
      }
    })
  }
)

// delete post
function deletePost(index){
  posts.value.splice(index,1);
}


</script>

<template>
  <nav class="navbar navbar-expand-lg navbar-dark bg-dark sticky-top">
    <div class="container">
      <a class="navbar-brand" href="#">BAT Blog</a>
      <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarSupportedContent"
        aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse" id="navbarSupportedContent">
        <ul class="navbar-nav me-auto mb-2 mb-lg-0">
          <li class="nav-item">
            <a class="nav-link active" aria-current="page" href="#">Home</a>
          </li>
          <li class="nav-item">
            <a class="nav-link" href="#">Link</a>
          </li>
        </ul>
        <ul class="navbar-nav mb-2 mb-lg-0">
          <li class="nav-item dropdown">
            <a class="nav-link dropdown-toggle" href="#" role="button" data-bs-toggle="dropdown" aria-expanded="false">
              John Doe
            </a>
            <ul class="dropdown-menu dropdown-menu-end">
              <li><a class="dropdown-item" href="#">Profile</a></li>
              <li><a class="dropdown-item" href="#">Logout</a></li>
            </ul>
          </li>
        </ul>
      </div>
    </div>
  </nav>
  <!-- create post start-->
  <div class="container mt-4">
    <div class="row">
      <div class="col-md-8 offset-md-2">
        <div class="card">
          <div class="card-body">
            <h5 class="card-title">Create a New Post</h5>
            <form @submit.prevent="createPost()">
              <div class="mb-3">
                <label for="title" class="form-label">Title</label>
                <input v-model="formData.title" type="text" class="form-control" id="title" placeholder="Enter the title">
              </div>
              <div class="mb-3">
                <label for="content" class="form-label">Content</label>
                <textarea v-model="formData.content" class="form-control" id="content" rows="3" placeholder="Enter the content"></textarea>
              </div>
              <button type="submit" class="btn btn-success">Submit</button>
            </form>
          </div>
        </div>
      </div>
    </div>
  </div>
  <!-- create post end-->

  <Posts :posts="posts" 
    @increaseLikeCount = "increaseLikeCount"
    @deletePost        = "deletePost"
   >
  </Posts>
</template>

<style scoped>
.cursor-pointer {
  cursor: pointer;
}

.very-low-opacity {
  opacity: 0.1;
}
</style>
