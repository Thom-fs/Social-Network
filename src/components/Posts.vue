<template>
  <div class="content">
    <div class="all" v-for="element in posts" :key="element._id">
      <div class="topComment">
        <div class="Profil">
          <img
            class="imageprofil"
            src="../assets/img/canardBocalacContours.png"
          />
          <div>
            <p class="firstname">{{ element.firstname }}</p>
            <p class="date">
              {{ new Date(element.date).toLocaleDateString() }}
            </p>
          </div>
        </div>
        <div>
          <button
            class="dot"
            @click="openSuppr = openSuppr === element._id ? null : element._id"
          >
            ● ● ●
          </button>
          <div>
            <button v-if="openSuppr === element._id" :id="element._id" class="">
              Suppr
            </button>
          </div>
        </div>
      </div>

      <div class="comment">
        <p>{{ element.content }}</p>
      </div>

      <div class="bottomComment">
        <div class="buttonlike">
          <button class="likeAndDislike" @click="addLike(element._id)">
            <img src="../assets/img/like48.png" />
          </button>
          <p>{{ element.likes.length }}</p>

          <button class="likeAndDislike" @click="addDislike(element._id)">
            <img class="image180" src="../assets/img/like48.png" />
          </button>
          <p>{{ dislike }}</p>
        </div>
        <div class="buttonBottomRight">
          <button
            @click="
              openCommentPostId =
                openCommentPostId === element._id ? null : element._id
            "
            class="buttonHover"
          >
            Commenter
          </button>
          <button class="buttonHover">Partager</button>
        </div>
      </div>

      <!-- affichage v-if ==> commentaires de posts -->
      <!-- :tableauComments="element.comments" == 
          passage en props ==> tableauComments: Array, -->
      <Commentaire
        v-if="openCommentPostId === element._id"
        :id="element._id"
        :tableauComments="element.comments"
        class=""
        @new-comment="getpost"
      />
    </div>
  </div>
</template>

<script>
import EditPost from "@/components/EditPost.vue";
import Commentaire from "@/components/Commentaire.vue";
export default {
  components: {
    EditPost,
    Commentaire,
  },

  name: "Posts",

  data() {
    return {
      comments: "",
      like: 0,
      dislike: 0,
      posts: [],
      page: 1,
      totalPages: 1,
      openCommentPostId: null,
      openSuppr: null,
    };
  },

  // afficher getpost une fois publié et recuperé
  mounted() {
    this.getpost();
  },

  // Ajouter un like
  methods: {
    async addLike(id) {
      const options = {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
          Authorization: "bearer " + localStorage.getItem("token"),
        },
        body: JSON.stringify({
          postId: id,
        }),
      };

      const response = await fetch(
        "https://social-network-api.osc-fr1.scalingo.io/bocalac/post/like",
        options
      );

      const result = await response.json();

      if (result.success === true) {
        this.getpost();
      }
    },

    // async addDislike(id) {
    //   const options = {
    //     method: "POST",
    //     headers: {
    //       "Content-Type": "application/json",
    //       Authorization: "bearer " + localStorage.getItem("token"),
    //     },
    //     body: JSON.stringify({
    //       postId: id,
    //     }),
    //   };

    //   const response = await fetch(
    //     "https://social-network-api.osc-fr1.scalingo.io/bocalac/post/like",
    //     options
    //   );

    //   const result = await response.json();

    //   if (result.success === true) {
    //     this.getpost();
    //   }
    // },

    // openCommentPostId(e){
    //   this. = e.target.value;
    // }

    // Récupérer tous les posts
    async getpost() {
      const options = {
        method: "GET",
        headers: {
          "Content-Type": "application/json",
        },
      };

      const response = await fetch(
        "https://social-network-api.osc-fr1.scalingo.io/bocalac/posts",
        options
      );

      const data = await response.json();
      this.posts = data.posts;

      console.log(data);
      console.log(this.posts);
    },
  },
};
</script>

<style scoped>
/*  ALL  */
.all {
  background-color: #474e58;
  color: white;
  border-radius: 10px;
  padding: 0px 20px;
  border: 3px solid white;
  box-shadow: 2px 4px 3px grey;
}
.content {
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.comment {
  padding-left: 5%;
}

/*  TOP  */
.topComment {
  display: flex;
  justify-content: space-between;
}
.imageprofil {
  width: 40px;
  height: 40px;
}
.Profil {
  display: flex;
  width: 20%;
  align-items: center;
  gap: 10px;
}

.firstname {
  font-size: 1.25em;
}

.date {
  color: #e0e0e0;
  font-size: 0.8em;
  position: relative;
  top: -12px;
}

.dot {
  color: #e0e0e0;
  background-color: #474e58;
  border: none;
  padding-top: 10px;
}

/*  BOT  */
.bottomComment {
  display: flex;
  justify-content: space-between;
}

.buttonlike {
  display: flex;
}

.image180 {
  transform: rotate(180deg);
  position: relative;
  top: 4px;
}

img {
  width: 20px;
  height: 20px;
}

.likeAndDislike {
  border: none;
  background: white;
  background-color: #474e58;
  cursor: pointer;
}

button {
  border-radius: 10px;
  border: 2px solid #5adfbc;
  background-color: white;
}

.buttonBottomRight {
  display: flex;
  align-items: center;
  gap: 5px;
}

.buttonHover:hover {
  border-radius: 10px;
  border: 2px solid #5adfbc;
  background-color: #5adfbc;
  color: white;
  cursor: pointer;
}

.button {
  padding: 0 2%;
  background: white;
  border: 2px solid #5adfbc;
  border-radius: 10px;
  color: #474e58;
  padding: 1% 3%;
}
</style>
