<template>
  <div class="modal-window">
    <span><i @click="closeCreatePopup" class="material-icons">close</i></span>
    <h2>Create new comment</h2>
    <div>
      <label for="name">Name</label>
      <input type="text" id="name" v-model="name" tabindex="1" />
    </div>
    <div>
      <label for="email">E-mail</label>
      <input type="email" id="email" v-model="email" tabindex="2" />
    </div>
    <div>
      <p>Comment</p>
      <textarea tabindex="3" v-model="body" cols="25" rows="6"></textarea>
    </div>
    <div class="modal-button">
      <button type="submit" @click="createComment">Add</button>
      <button type="reset" @click="reset">Reset</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "CreateComment",
  props: ["createNewComment", "length"],
  data() {
    return {
      name: "",
      email: "",
      body: "",
      id: 0,
    };
  },
  methods: {
    closeCreatePopup() {
      this.$emit("closeCreatePopup");
    },

    createComment() {
      if (this.name != "" && this.email != "" && this.body != "") {
        this.id = this.length();
        this.createNewComment({
          id: this.id,
          name: this.name,
          email: this.email,
          body: this.body,
        });

        (this.name = ""), (this.email = ""), (this.body = "");
      }
    },
    reset() {
      (this.name = ""), (this.email = ""), (this.body = "");
    },
  },
};
</script>
<style scoped>
.modal-window {
  position: fixed;
  top: 110px;
  right: 80px;
  display: flexbox;
  align-items: center;
  justify-content: center;
  background: rgb(255, 255, 255);
  padding: 30px;
  z-index: 10;
  box-shadow: 10px 5px 5px rgba(0, 0, 0, 0.2);
}

.modal-window input {
  margin: 10px;
  padding: 5px;
}
.modal-button {
  margin: 10px;
}
.hide {
  display: none;
}
span {
  position: fixed;
  right: 100px;
  top: 125px;
}
</style>
