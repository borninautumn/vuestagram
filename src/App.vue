<template>
  <div class="header">
    <ul class="header-button-left">
      <li v-if="step != 0" @click="step = 0">Cancel</li>
    </ul>
    <ul class="header-button-right">
      <li v-if="step == 1" @click="step++">Next</li>
      <li v-if="step == 2" @click="Publish">발행</li>
    </ul>
    <img src="./assets/logo.png" class="logo" />
  </div>

  <!-- <h4>안녕 {{ $store.state.name }}</h4> store.js에 있는 state 꺼내쓰는 방법
  <button @click="$store.state.name = '박' ">버튼</button> 이렇게 직접 수정하면 안됨! store.js에 부탁할거임
  <button @click="$store.commit('이름변경')">버튼</button>
  <h4>나이는 {{ $store.state.age }}</h4>
  <button @click="$store.commit('나이먹기')">버튼2</button>
  <button @click="$store.commit('나이십씩먹기', 10)">버튼3</button> 10씩 증가시키고 싶다면? -->

  <!-- 더보기게시물 ajax 요청하려면?
    commit은 mutations 부탁
    dispatch는 actions 부탁
   -->
  <!-- <p>{{ $store.state.more }}</p> -->
  <button v-if="step == 0" @click="$store.dispatch('getData')">더보기버튼</button>

  <Container 
    :게시물="게시물" 
    :step="step" 
    :이미지url="이미지url" 
    @write="작성한글 = $event"
  />

  <!-- vuex 예시 -->
  <!-- <p>{{ Now() }} {{ 카운터 }}</p> -->
  <!-- <p>{{ Now2 }} {{ 카운터 }}</p>
  <button @click="카운터++">버튼</button>
  <p>{{ name }}</p> -->

  <button v-if="step == 0" @click="more">더보기</button>

  <div v-if="step == 0" class="footer">
    <ul class="footer-button-plus">
      <input @change="upload" type="file" id="file" class="inputfile" />
      <label for="file" class="input-plus">+</label>
    </ul>
 </div>

 <!-- <div v-if="step == 0">내용0</div>
 <div v-if="step == 1">내용1</div>
 <div v-if="step == 2">내용2</div>
 <button @click="step = 0">버튼0</button>
 <button @click="step = 1">버튼1</button>
 <button @click="step = 2">버튼2</button> -->
 
</template>

<script>
import Container from './components/ContainerComponent.vue'
import post from './assets/post'
import axios from 'axios' // axios로 ajax 요청하는 법
// axios.get() // 내가 원하는 url로 get요청할 수 있음
import {mapMutations, mapState} from 'vuex'

export default {
  name: "App",
  data() {
    return {
      선택한필터: '',
      작성한글: '',
      이미지url: '', // url 변수 등록
      step: 3, // App.vue에 현재 페이지 상태를 저장함
      // step: 0, // 0이면 내용 0을 보여주고 1이면 내용 1을 보여줌
      게시물: post, // 글 발행 기능 만들기 -> <Post />를 하나 더 만들려면? 데이터만 건들면 된다.
      더보기: 0,
      카운터: 0,
    }
  },
  mounted() {
    // this.emitter.on('작명', (a) => { // 누가 '작명'이란 이벤트 발사하면
    //   // 이 코드 실행해주셈
    //   console.log(a)
    // })
    this.emitter.on('박스클릭함', (a) => { // 누가 '작명'이란 이벤트 발사하면
      // 이 코드 실행해주셈
      this.선택한필터 = a
    })
  },
  components: {
    Container,
  },

  computed: { // 사용해도 실행되지 않음, 처음 실행하고 값을 간직함, 계산 결과 저장용 함수임
    // Now2() { return new Date() },
    // name() { return this.$store.state.name },
    // vuex state 한번에 꺼내쓰려면? ...mapState(['state이름'])
    ...mapState(['name', 'age', 'likes']),
    // 오브젝트 자료형도 가능 ...mapState({작명: 'state이름'})
    ...mapState({
      작명: 'name',
    }),
  },

  methods: { // 사용할때마다 실행됨
    // Now() { return new Date() },
    // vuex mutations 한번에 쓰려면? ...mapMutations(['함수명'])
    ...mapMutations(['setMore', 'UpLike']),
    Publish() { // 발행버튼 누르면? this.게시물에 { 내가쓴거 } 밀어넣기
      var 내게시물 = {
        name: "Kim Hyun",
        userImage: "https://placeimg.com/100/100/arch",
        postImage: this.이미지url,
        likes: 36,
        date: "May 15",
        liked: false,
        content: this.작성한글,
        // filter: "perpetua"
        filter: this.선택한필터
      };
      this.게시물.unshift(내게시물); // 왼쪽의 array에 자료를 집어넣어줌
      this.step = 0; // 메인페이지로 돌아감
    },
    more() {
      // axios 사용법 더 알아보자면
      // axios.post('URL', {name: 'kim'}) // URL로 내가 원하는 정보를 보낼 때 사용
      // .then() // 요청이 성공했을 때 실행할 코드
      // .catch((err) => { err.message }) // 요청이 실패했을 때 실행할 코드
      // axios.get('URL').then(get요청이성공했을때실행)
      axios.get(`https://codingapple1.github.io/vue/more${this.더보기}.json`)
      .then((결과) => {
        console.log(결과.data);
        this.게시물.push(결과.data); // array에 데이터를 추가함
        this.더보기++;
      })
    },
    upload(e) {
      let 파일 = e.target.files;
      // console.log(파일[0]);
      // 업로드 후엔 다음 페이지로 보내야 함
      let url = URL.createObjectURL(파일[0]); 
      console.log(url);
      this.이미지url = url; // 데이터로 등록한 이미지url에 경로데이터를 넣음
      this.step = 1;
      // 업로드한 이미지를 띄워야 함
    },
  },
};
</script>

<style>
body {
  margin: 0;
}
ul {
  padding: 5px;
  list-style-type: none;
}
.logo {
  width: 22px;
  margin: auto;
  display: block;
  position: absolute;
  left: 0;
  right: 0;
  top: 13px;
}
.header {
  width: 100%;
  height: 40px;
  background-color: white;
  padding-bottom: 8px;
  position: sticky;
  top: 0;
}
.header-button-left {
  color: skyblue;
  float: left;
  width: 50px;
  padding-left: 20px;
  cursor: pointer;
  margin-top: 10px;
}
.header-button-right {
  color: skyblue;
  float: right;
  width: 50px;
  cursor: pointer;
  margin-top: 10px;
}
.footer {
  width: 100%;
  position: sticky;
  bottom: 0;
  padding-bottom: 10px;
  background-color: white;
}
.footer-button-plus {
  width: 80px;
  margin: auto;
  text-align: center;
  cursor: pointer;
  font-size: 24px;
  padding-top: 12px;
}
.sample-box {
  width: 100%;
  height: 600px;
  background-color: bisque;
}
.inputfile {
  display: none;
}
.input-plus {
  cursor: pointer;
}
#app {
  box-sizing: border-box;
  font-family: "consolas";
  margin-top: 60px;
  width: 100%;
  max-width: 460px;
  margin: auto;
  position: relative;
  border-right: 1px solid #eee;
  border-left: 1px solid #eee;
}
</style>
