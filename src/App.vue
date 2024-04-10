<template>
  <div class="header">
    <ul class="header-button-left">
      <li @click="confirm">Cancel</li>
    </ul>
    <ul class="header-button-right">
      <li v-if="step === 1" @click="step += 1">Next</li>
      <li v-if="step === 2" @click="post">발행</li>
    </ul>
    <img src="./assets/logo.png" class="logo" />
  </div>

  <Container
    :instaData="instaData"
    :step="step"
    :uploadImage="uploadImage"
    @content-update="content = $event"
  />
  <button
      type="button"
      @click="more"
  >더보기</button>

  <div class="footer">
    <ul class="footer-button-plus">
      <input @change="upload" type="file" id="file" class="inputfile" />
      <!-- multiple: 여러 파일 받을 수 있음 / accept="image/*": 허용가능한 파일 목록 설정 -->
      <label for="file" class="input-plus">+</label>
    </ul>
  </div>

</template>

<script>
import Container from "@/components/Container.vue";
import instaData from "@/assets/instaData";
import axios from "axios";

export default {
  name: 'App',
  data(){
    return{
      instaData: instaData, // instaData
      btnclicked: 0,
      step: 0,
      uploadImage : '',
      content: '',
    }
  },
  components: {
    Container
  },
  methods: {
    more(){
      // axios.post('URL', {name: 'kim'}).then().catch()
      // URL에 {name: 'kim'}을 전달한다

      if(this.btnclicked >= 2){
        return false;
      }else {
        axios.get(`https://codingapple1.github.io/vue/more${this.btnclicked}.json`)
        .then(result => {
          this.instaData.push(result.data);
          this.btnclicked ++;
        })
      }
    },
    upload(e){
      let imgFile = e.target.files;
      this.step = 1;
      let fileUrl = URL.createObjectURL(imgFile[0]);
      this.uploadImage = fileUrl;
    },
    confirm(){
      if(window.confirm('처음으로 돌아가시겠습니까?')){
        this.step = 0
      }
    },
    post(){
      let newPost = {
        name: "Minny",
        userImage: "https://picsum.photos/100?random=5",
        postImage: this.uploadImage,
        likes: 49,
        date: "Apr 4",
        liked: false,
        content: this.content,
        filter: "lofi",
      };
      this.instaData.unshift(newPost);  // 맨 앞으로 추가
      this.step = 0;
    },
  }
}
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
