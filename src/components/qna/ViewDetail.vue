<template>
  <div class="regist" id="font">
    <h1 class="underline">게시글 정보</h1>
    <div class="regist_form">
      <label for="isbn">번호</label>
      <div class="view">{{ book.isbn }}</div>
      <label for="title">제목</label>
      <div class="view">{{ book.title }}</div>
      <label for="author">글쓴이</label>
      <div class="view">{{ book.author }}</div>
      <label for="writtendate">작성일</label>
      <div class="view">{{ book.writtendate }}</div>
      <label for="content">내용</label>
      <div class="view" v-html="enterToBr(book.content)"><textarea></textarea></div>
      <div style="padding-top: 15px">
        <router-link :to="`/qna/modify/${book.isbn}`" class="btn"><button id="btn_group" class="btn">수정</button></router-link>
        <a href="#" class="btn" @click="deleteBook"><button id="btn_group" class="btn">삭제</button></a>
        <router-link to="/qna/qna" class="btn"><button id="btn_group" class="btn">목록</button></router-link>
      </div>
    </div>
  </div>
</template>

<script>
import http from "@/util/http-common";
import { mapGetters } from 'vuex';

export default {
  name: "viewdetial",
  computed: {
      ...mapGetters(["book"])
  },

  data() {
    return {
      isbn: "",
      title: "",
      author: "",
      writtendate: "",
      content: ""
    };
  },
  created() {
    if (this.type === "modify") {
      http.get(`/post/${this.$route.params.isbn}`).then(({ data }) => {
        this.isbn = data.isbn;
        this.title = data.title;
        this.author = data.author;
        this.writtendate= data.writtendate;
        this.content = data.content;
        this.hit = this.hit + 1;
      });
    }
  },
  methods: {
    deleteBook() {
      if (confirm("정말로 삭제?")) {
        http.delete(`/post/${this.book.isbn}`).then(({ data }) => {
          let msg = "삭제 처리시 문제가 발생했습니다.";
          if (data === "success") {
            msg = "삭제가 완료되었습니다.";
          }
          alert(msg);
          this.$router.push("/qna/qna");
        });
      }
    },
    numberWithCommas(x) {
      if (x) return x.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",");
    },
    enterToBr(str) {
      if (str) return str.replace(/(?:\r\n|\r|\n)/g, "<br />");
    }
  }
};
</script>
<style scoped>
.regist {
  padding: 10px;
}
.regist_form {
  text-align: left;
  border-radius: 5px;
  background-color: #f2f2f2;
  padding: 20px;
}
input,
textarea,
.view {
  width: 100%;
  padding: 12px 20px;
  margin: 8px 0;
  display: inline-block;
  border: 1px solid #ccc;
  border-radius: 4px;
  box-sizing: border-box;
  color: #787878;
  font-size: medium;
}
#btn_group { 
  border: 1px solid skyblue; 
  background-color: rgba(0,0,0,0); 
  color: skyblue; padding: 5px; 
  width: 150px;
}

#btn_group:hover { 
  color : white;
  background-color: skyblue;
}


</style>
