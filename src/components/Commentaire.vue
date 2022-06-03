<template>
  <div>
    <div class="publier">
      <input
        v-model="postCommentaire"
        id=""
        type="text"
        placeholder="Ecrivez votre commentaire"
      />
      <button @click="postComment()">Publier</button>
    </div>
    <div class="scroll-div">
      <!-- boucle des commentaires (element.content) 
      dans le tableauComments contenant tous les comments ==> content(commentaires)-->
      <div v-for="element in tableauComments" :key="element._id">
        <p>
          <span>{{ element.firstname }} : </span> {{ element.content }}
        </p>
      </div>
    </div>
  </div>

  <!-- 
  element.comments.content -->
</template>

<script>
export default {
  data() {
    return {
      postCommentaire: "",
      token: localStorage.getItem("token"),
      listCommentaires: [],
    };
  },

  emits: ["new-comment"],

  props: {
    id: String,
    tableauComments: Array,
  },

  methods: {
    // Créer un commentaire
    // fucnction promesse > publication de commentaires > para id de chaque post
    // para id appelé @click="postComment() sur le bouton publier
    async postComment() {
      const options = {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
          Authorization: "bearer " + localStorage.getItem("token"),
        },
        body: JSON.stringify({
          postId: this.id,
          content: this.postCommentaire,
        }),
      };

      const response = await fetch(
        "https://social-network-api.osc-fr1.scalingo.io/bocalac/post/comment",
        options
      );

      const data = await response.json();
      this.postCommentaire = "";

      console.log(response);
      console.log(data);

      this.$emit("new-comment");
    },
  },
};
</script>

<style scoped>
.scroll-div {
  height: fit-content;
  max-height: 220px;
  overflow-y: scroll;
}

.publier {
  padding-bottom: 7px;
}

button {
  cursor: pointer;
}
input {
  border-radius: 5px;
}

p {
  border: 2.5px solid #5adfbc;
  border-radius: 7px;
  width: fit-content;
  padding: 0.5% 1%;
  background-color: white;
  color: #474e58;
}
span {
  font-weight: bold;
}

button {
  border-radius: 10px;
  border: 2px solid #5adfbc;
  background-color: white;
}

button:hover {
  border-radius: 10px;
  border: 2px solid #5adfbc;
  background-color: #5adfbc;
  color: white;
  cursor: pointer;
}
</style>
