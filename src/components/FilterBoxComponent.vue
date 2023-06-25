<template>
  <div @click="fire" :class="필터" class="filter-item" :style="`background-image: url(${이미지}) `">
    <!-- 
      slot으로 부모 -> 자식 데이터 전송 가능 
      1. 자식은 구멍 뚫기 -> <slot></slot>으로
      2. 부모 컴포넌트에서 <컴포넌트> 태그 사이에 데이터 넣기
      slot은 HTML 태그 안에 데이터바인딩할 때만 slot을 사용한다.
      -->
    <slot></slot>
    <!-- 
      mitt로 데이터 전송하는 법
      1. this.emitter.emit() 으로 발사하고
      2. this.emitter.on() 으로 수신하면 됨 -> 수신하는 코드는 mounted 안에 작성하는 게 관습적임
     -->
    <!-- <button @click="fire">버튼</button> -->
  </div> 
  <!-- <div :class="필터 + ` filter-item`" :style="`background-image: url(${이미지}) `"></div>  -->
  <!-- <div :class="`${필터} filter-item`" :style="`background-image: url(${이미지}) `"></div>  -->
</template>

<script>
export default {
  name: 'FilterBoxComponent', // 이름 아무렇게나 작명 가능
  props: {
    이미지: String,
    필터: String,
  },
  methods: {
    fire() {
      // this.emitter.emit('작명', '데이터'); // 이벤트 쏴주는 법
      this.emitter.emit('박스클릭함', this.필터);
    }
  },
}
</script>

<style>
.filter-item {
  width: 100px;
  height: 100px;
  margin: 10px 10px 10px auto;
  padding: 8px;
  display: inline-block;
  color : white;
  background-size: cover;
  background-position : center;
}
</style>