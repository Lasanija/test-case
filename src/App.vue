<template>
  <div id="app">
    <div class="container">
      <div class="header">
        <h1>Comments</h1>
        <div class="headerForm">
          <input type="text" placeholder="Find some" v-model="search" />
          <button type="submit" @click="showPopupCreate">Create</button>
          <create-comment
            v-if="isCreatePopupVisible"
            @closeCreatePopup="closeCreatePopup"
            :createNewComment="createNewComment"
            :length="length"
          ></create-comment>
        </div>
      </div>
      <div class="table">
        <div class="tableHead">
          <div class="commentId">ID</div>
          <div class="commentName">NAME</div>
          <div class="commentEmail">EMAIL</div>
          <div class="commentBody">COMMENT</div>
        </div>
        <div
          class="tableBody"
          v-for="(comment, index) in filterComments"
          :key="index"
          @click="selected(comment)"
        >
          <div class="commentId">{{ comment.id }}</div>
          <div class="commentName">{{ comment.name }}</div>
          <div class="commentEmail">{{ comment.email }}</div>
          <div class="commentBody">{{ comment.body }}</div>
        </div>
      </div>
      <div class="infoPopup" v-if="showInfo">
        <span @click="hideInfo"><i class="material-icons ">close</i></span>
        <h2>Comment info</h2>
        <p>Name :{{infoName}}</p>
        <p>E-mail: {{infoEmail}}</p>
        <p>Comment :{{infoComment}}</p>
        <button class="infoButton" @click="deleteComment()">delete</button>
      </div>
    </div>
  </div>
</template>

<script>
import CreateComment from "./components/CreateComment.vue";
export default {
  name: "App",
  components: {
    CreateComment,
  },
  data() {
    return {
      comments: [],
      search: "",
      isCreatePopupVisible: false,
      showInfo: false,
      select:'',
      infoName:'',
      infoEmail:'',
      infoComment:'',
    };
  },
  mounted() {
    this.axios
      .get("https://jsonplaceholder.typicode.com/comments")
      .then((response) => (this.comments = response.data.slice(0, 20)))
      .catch((error) => console.log(error));
  },
  methods: {
    showPopupCreate() {
      return (this.isCreatePopupVisible = true);
    },
    closeCreatePopup() {
      return (this.isCreatePopupVisible = false);
    },
    createNewComment(data) {
      this.comments.push(data);

      // для отправки данных на сервер
      // this.axios
      // .post("https://jsonplaceholder.typicode.com/comments", this.newComments)
      // .then((response) => {
      //   console.log(response);
      // })
      // .catch((error) => {
      //   console.log(error);
      // });
    },
    length() {
      let idArr=[]
      this.comments.forEach(element => {
        this.idArr.push(element.id)
      });
      let newId = Math.max.apply(idArr)
      return newId+1
    },
    hideInfo() {
      return (this.showInfo = false);
    },
    selected(sel){
      this.showInfo = true;
      this.select= this.comments.indexOf(sel)
      this.infoName = this.comments[this.select].name
      this.infoEmail = this.comments[this.select].email
      this.infoComment = this.comments[this.select].body

    },
    deleteComment(){
      confirm('Do you want delete '+this.infoEmail+'?')
this.comments.splice(this.select,1)
this.showInfo = false;
    }
  },
  computed: {
    filterComments() {
      return this.comments.filter((comment) => {
        // return comment.email.indexOf(this.search) > -1; - учитывает регистр
        let reg = new RegExp(`${this.search}`, "gi");
        return comment.email.match(reg);
      });
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #060029;
}
.tableBody,
.tableHead {
  justify-items: flex-start;
  align-items: center;
  display: grid;
  grid-template-columns: 0.25fr 1fr 1fr 1.75fr;
  grid-template-rows: 1fr;
  gap: 1px 10px;
}
.tableBody,
.tableHead{
  border-bottom: 1px solid rgb(3, 3, 66);
}
.tableHead{
  padding-bottom:15px;
  font-weight: 600;
}
.header {
  margin-bottom: 30px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}
button {
  color: azure;
  background: rgb(3, 3, 66);
  border: none;
  margin: 0px 15px;
  height: 30px;
  width: 80px;
}
.infoPopup {
  position: fixed;
  top: 110px;
  right: 400px;
  display: flexbox;
  align-items: center;
  justify-content: center;
  background: rgb(255, 255, 255);
  padding: 30px;
  box-shadow: 10px 5px 5px rgba(0, 0, 0, 0.2);
  width: 400px;
  border: 1px solid rgb(3, 3, 66);
}
.infoPopup p{
  border-bottom:2px solid rgb(3, 3, 66);
}
.infoPopup span {
  position: fixed;
  right: 420px;
  top: 125px;
  box-shadow: none;
}
</style>
