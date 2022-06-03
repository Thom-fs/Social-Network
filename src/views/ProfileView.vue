<template>
  <div class="contentProfilView">
    <div class="infosProfil">
      <div class="pdp">
        <img class="canard" src="../assets/img/CanardBocalac.png" />
      </div>
      <div class="textInfosProfil">
        <p>{{ nom }} {{ prenom }}</p>
        <div class="statut">
          <p>Statut:</p>
          <div class="pastilleStatut"></div>
        </div>
        <p>Ceci est une petite description / biographie</p>
      </div>
    </div>
    <Postcontainer />
  </div>
</template>

<script>
import HeaderBar from "@/components/HeaderBar.vue";
import Postcontainer from "@/components/Postcontainer.vue";
import AsideBar from "@/components/AsideBar.vue";

export default {
  components: {
    HeaderBar,
    Postcontainer,
    AsideBar,
  },

  props: {
    nom: String,
    prenom: String,
  },

  data() {
    return {
      lastname: this.nom,
      firstname: this.prenom,
      email: this.mail,
    };
  },
};
</script>

<style scoped>
.infosProfil {
  display: flex;
  gap: 20px;
  margin: 3%;
}

.textInfosProfil {
  display: flex;
  flex-direction: column;
  gap: 10px;
  justify-content: center;
}

.pdp {
  width: 110px;
  height: 110px;
  border: 2px solid black;
  border-radius: 50%;
  background-color: bisque;
  cursor: pointer;
  display: flex;
  justify-content: center;
  align-items: center;
}

.statut {
  display: flex;
  align-items: center;
  gap: 10px;
}

.pastilleStatut {
  background-color: #06d406;
  width: 10px;
  height: 10px;
  border-radius: 10px;
}

p {
  cursor: pointer;
  margin: 0;
}

.contentProfilView {
  padding: 2%;
}

button {
  position: relative;
  float: right;
  right: 20%;
  top: -20px;
}
h1 {
  color: #474e58;
}
.containerProfile {
  width: 85%;
}
.mainContainerProfile {
  display: flex;
  position: relative;
  top: 8px;
}
.canard {
  width: 80px;
  height: 80px;
  position: relative;
}
</style>
