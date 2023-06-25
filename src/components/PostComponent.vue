<template>
  <!-- <div class="post" v-for="(포스트, i) in 게시물" :key="i">
    <div class="post-header">
      <div class="profile" :style="{ backgroundImage: `url(${포스트.userImage})`}"></div>
      <span class="profile-name">{{ 포스트.name }}</span>
    </div>
    <div @click="$store.commit('UpLike')" 
      :class="포스트.filter" class="post-body" 
      :style="{ backgroundImage: `url(${포스트.postImage})` }"></div>
    <div class="post-content">
      <p>{{ 포스트.likes }} Likes</p>
      <p>{{ $store.state.likes }} Likes</p>
      <p><strong>{{ 포스트.filter }}</strong> {{ 포스트.content }}</p>
      <p class="date">{{ 포스트.date }}</p>
    </div>
  </div> -->

  <!-- 컨테이너 컴포넌트에서 반복문을 돌렸을 때 post -->
  <div class="post">
    <div class="post-header">
      <div class="profile" :style="{ backgroundImage: `url(${게시물.userImage})`}"></div>
      <span class="profile-name">{{ 게시물.name }}</span>
    </div>
    <div @click="$store.commit('UpLike', i)" class="post-body" :style="{ backgroundImage: `url(${게시물.postImage})` }"></div>
    <div class="post-content">
      <p>{{ 게시물.likes }} Likes {{ 게시물.liked ? '♥' : '♡' }}</p>
      <p><strong>{{ 게시물.filter }}</strong> {{ 게시물.content }}</p>
      <p class="date">{{ 게시물.date }}</p>
    </div>
  </div>

</template>

<script>
export default {
  name: 'PostComponent',
  props: {
    게시물: Object,
    i: Number,
  },
}
</script>

<style>
.post {
  width: 100%;
}
.profile {
  background-image: url("https://placeimg.com/100/100/arch");
  width: 30px;
  height: 30px;
  background-size: 100%;
  border-radius: 50%;
  float: left;
}
.profile-name {
  display: block;
  float: left;
  padding-left: 10px;
  padding-top: 7px;
  font-size: 14px;
}
.post-header {
  height: 30px;
  padding: 10px;
}
.post-body {
  background-image: url("https://placeimg.com/640/480/animals");
  height: 450px;
  background-position: center;
  background-size: cover;
}
.post-content {
  padding-left: 15px;
  padding-right: 15px;
  font-size: 14px;
}
.date {
  font-size: 11px;
  color: grey;
  margin-top: -8px;
}
</style>