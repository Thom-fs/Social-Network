<template>
  <div class="editPost">
    <input v-model="post" type="text" placeholder="Ecrivez votre post" />
    <div class="buttons">
      <p>{{ fileName }}</p>
      <div v-if="fileUrl" class="previewFile">
        <img :src="fileUrl" alt="" />
      </div>
      <div class="img">
        <input @change="addFile" id="file" class="inputFile" type="file" />
        <label for="file" class="trombone">
          <img src="../assets/img/trombone48.png" alt="" />
        </label>
      </div>
      <button @click="publish">Publier</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      fileUrl: null,
      fileName: null,
      title: "",
      post: "",
      result: null,
      token: localStorage.getItem("token"),
    };
  },
  methods: {
    addFile(e) {
      let file = e.target.files[0];
      this.fileUrl = URL.createObjectURL(file);
      let nameFile = e.target.files[0].name;
      this.fileName = nameFile;
    },
    async publish() {
      const options = {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
          Authorization: "bearer " + localStorage.getItem("token"),
        },
        body: JSON.stringify({
          title: "toto",
          content: this.post,
        }),
      };

      const response = await fetch(
        "https://social-network-api.osc-fr1.scalingo.io/bocalac/post",
        options
      );

      const data = await response.json();
      this.post = "";

      console.log(response);
      console.log(data);
      window.location.reload();
    },
  },
};
</script>

<style scoped>
.editPost {
  display: Flex;
  flex-direction: column;
  padding: 2%;
  align-items: center;
  background-color: #474e58;
  border-radius: 10px;
  border: 3px solid white;
  box-shadow: 2px 4px 3px grey;
  gap: 20px;
}

input {
  width: 98%;
  padding: 1%;
  border: 2px solid black;
  border-radius: 5px;
  color: #474e58;
}

.buttons {
  width: 100%;
  display: flex;
  justify-content: flex-end;
  gap: 20px;
  align-items: center;
}

.buttons p {
  padding-top: 10px;
  color: white;
}

.inputFile {
  display: none;
}

.editPost input {
  outline: none;
  border: 2px solid white;
}

.img {
  width: 34px;
  height: 34px;
  background: white;
  border-radius: 50%;
  padding: 5px;
}

.trombone {
  position: relative;
  display: flex;
  width: 30px;
  background: white;
  border-radius: 50%;
  padding: 5px;
  border: 2px solid #5adfbc;
  bottom: 5px;
  right: 5px;
  cursor: pointer;
}

img {
  width: 100%;
}

.previewFile {
  width: 40px;
  height: 40px;
  background: white;
  padding: 5px;
  border: 2px solid #5adfbc;
}

button {
  padding: 0 2%;
  background: white;
  border: 2px solid #5adfbc;
  border-radius: 10px;
  color: #474e58;
  padding: 1% 3%;
  cursor: pointer;
}

button:hover {
  border: 2px solid #5adfbc;
  background: #5adfbc;
  color: white;
}
</style>
