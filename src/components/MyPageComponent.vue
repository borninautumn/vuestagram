<template>
  <div style="padding : 10px">
    <h4>팔로워</h4>
    <input placeholder="?" @input="search($event.target.value)" />
    <div v-for="(팔로워, i) in follower" :key="i" class="post-header">
      <div class="profile" :style="{ backgroundImage: `url(${팔로워.image})`}"></div>
      <span class="profile-name">{{ 팔로워.name }}</span>
    </div>
  </div>
</template>

<script>
import { onMounted, reactive, ref, toRefs } from 'vue'
import axios from 'axios';

export default {
  name: 'MyNameComponent',
  props: {
    one: Number,
  },
  setup(props) {
    // Composition API 써서 개발하려면 여기 안에 코드 짜셈
    // 데이터 생성, 조작, methods, hook 등등 생성 조작 다 가능
    // setup() 안에서 데이터 생성하는 법 : ref(데이터)
    // 데이터는 ref() 여기에 담고 return {} 해줘야 위에서 사용 가능
    let follower = ref([]);
    let test = reactive({name : 'kim'})
    test;
    let {one} = toRefs(props);
    console.log(one.value);
    let followerOriginal = ref([]);

    // Composition API에서 라이프사이클 훅 쓰려면 onMounted(()=>{실행할코드})로 써야함
    onMounted(() => {
      // ajax 요청, 데이터 변경 하려면?
      axios.get('/follower.json').then((a) => {
        follower.value = a.data // 데이터 수정하려면 데이터.value로 수정해야함
        followerOriginal.value = [...a.data];
      })
    })

    function search(검색어) {
      let newFollower = followerOriginal.value.filter((a) => {
        return a.name.indexOf(검색어) != -1
      });
      follower.value = [...newFollower]
    }

    return { follower, search }
  }
}
</script>

<style scoped>

</style>