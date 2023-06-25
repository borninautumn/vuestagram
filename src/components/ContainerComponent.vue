<template>
  <div>
    <div v-if="step == 0">
      <!-- <Post :게시물="게시물" /> -->
      <Post :게시물="$store.state.post[i]" v-for="(a, i) in $store.state.post" :key="i" :i="i"></Post>
    </div>
    <!--
      반복문 쓰는 방법.
      나는 그냥 Post에서 했는데 여기서 해도됨 
      <Post :게시물="게시물[i]" v-for="(a, i) in 게시물" :key="i" /> 
      <Post :게시물="a" v-for="(a, i) in 게시물" :key="i" />
    -->
    <!-- <Post/>
    <Post/> -->

    <!-- 저번시간에 만든거 -->
    <!-- <Post :게시물="게시물[i]" v-for="(a, i) in 게시물" :key="i" /> -->

    <!-- 필터선택페이지 -->
    <div v-if="step == 1">
      <div :class="선택한필터" class="upload-image" :style="`background-image: url(${이미지url})`"></div>
      <div class="filters">
        <FilterBox :필터="필터" :이미지="이미지url" v-for="(필터, i) in 필터들" :key="i">
          {{ 필터 }}
        </FilterBox>
      </div>
    </div>

    <!-- 글작성페이지 -->
    <div v-if="step == 2">
      <div :class="선택한필터" class="upload-image" :style="`background-image: url(${이미지url})`"></div>
      <div class="write">
        <textarea @input="$emit('write', $event.target.value)" class="write-box">write!</textarea>
      </div>
    </div>
  </div>

  <!-- 팔로워 목록 -->
  <div v-if="step == 3">
    <MyPage :one="1" />
  </div>
</template>

<script>
import Post from './PostComponent.vue'
import FilterBox from './FilterBoxComponent.vue'
import MyPage from './MyPageComponent.vue'

export default {
  name: 'ContainerComponent',
  data() {
    return {
      필터들: [ "aden", "_1977", "brannan", "brooklyn", "clarendon", "earlybird", "gingham", "hudson", 
"inkwell", "kelvin", "lark", "lofi", "maven", "mayfair", "moon", "nashville", "perpetua", 
"reyes", "rise", "slumber", "stinson", "toaster", "valencia", "walden", "willow", "xpro2"],
      선택한필터: '',
    }
  },
  props: {
    이미지url: String,
    step: Number,
    게시물: Array,
  },
  mounted() {
    this.emitter.on('박스클릭함', (a) => { // 누가 '작명'이란 이벤트 발사하면
      // 이 코드 실행해주셈
      this.선택한필터 = a
    })
  },
  components: {
    Post,
    FilterBox,
    MyPage,
  },
}
</script>

<style>
.upload-image{
	width: 100%;
	height: 450px;
	background: cornflowerblue;
	background-size : cover;
}
.filters{
	overflow-x:scroll;
	white-space: nowrap;
}
.filter-1 {
	width: 100px;
	height: 100px;
	background-color: cornflowerblue;
	margin: 10px 10px 10px auto;
	padding: 8px;
	display: inline-block;
	color : white;
	background-size: cover;
}
.filters::-webkit-scrollbar {
	height: 5px;
}
.filters::-webkit-scrollbar-track {
	background: #f1f1f1; 
}
.filters::-webkit-scrollbar-thumb {
	background: #888; 
	border-radius: 5px;
}
.filters::-webkit-scrollbar-thumb:hover {
	background: #555; 
}
.write-box {
	border: none;
	width: 90%;
	height: 100px;
	padding: 15px;
	margin: auto;
	display: block;
	outline: none;
}
</style>