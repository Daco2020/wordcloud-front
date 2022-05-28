<template>
<div>
  <h2 class=title>벨로그 워드클라우드</h2>
  <div class=sub>
  <input v-model="id" placeholder="아이디를 입력해주세요."> 
  <!-- <input v-model="limit" placeholder="몇 개의 글을 분석할까요?">  -->
  <button @click="fetch">분석하기</button>
  </div>
  <p> https://velog.io/@{{id}} </p>
  <p>{{message}}</p>
  <img :src=wordCloud>
</div>
</template>

<script>
import axios from "axios"

export default {
  name: 'App',
  data(){
    return {
      id : "",
      wordCloud : "",
      message : "",
    }
  },
  methods : {
    async fetch(){
      this.message = "최근 글 3개를 분석하고 있습니다."
      await axios({
        method: "GET",
        url:"http://3.17.75.87:8000/search/"+this.id, 
        responseType: 'arraybuffer',
      })
      .then((res)=>{
        console.log(res)
        let blob = new Blob([res.data], {type: res.headers['content-type']});
        let image = URL.createObjectURL(blob)
        this.message = "분석을 완료했습니다."
        this.wordCloud = image
      })
      .catch(e => {
        console.log(`error === ${e}`)
        this.message = "다시 시도해주세요..."
      })
  }

  }
  ,
  components: {
  }
}
</script>

<style>
.title {
  margin : 30px 0px 30px 0px;
  padding : 0px;
  /* background: #7bd2ef; */
}
.sub {
  margin: 0px;
  padding: 0px;
}
p {
  margin-top: 10px;
  margin-bottom: 0px;
}
img {
  margin-top: 0px;
}
body {
  margin : 0px
}
div {
  box-sizing: border-box;
}


#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

</style>
