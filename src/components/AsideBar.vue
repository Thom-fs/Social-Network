<template>
  <aside class="containerFriends">
    <section id="connectedFriends">
      <p class="titleAside">Amis connectés</p>
      <div class="rondFlex">
        <div class="rond canard1"></div>
        <div class="rond canard2"></div>
        <div class="rond canard3"></div>
        <div class="rond plus">+</div>
      </div>
    </section>
    <section id="friends"></section>
    <p class="titleAside">Amis</p>
    <div class="rondFlex">
      <div class="rond canard4"></div>
      <div class="rond canard5"></div>
      <div class="rond canard6"></div>
      <div class="rond plus">+</div>
    </div>
  </aside>
</template>

<script>
export default {
  setup() {},
};
</script>

<style scoped>
.mainContainerProfile {
  display: flex;
  position: relative;
  top: 8px;
}

.titleAside {
  color: #474e58;
  padding: 20px 20px 5px 20px;
  margin: 0px;
}

.containerProfile {
  width: 85%;
}

.containerFriends {
  background-color: #e0e0e0;
  width: 15%;
  border-left: 4px solid #5adfbc;
}

.rond {
  border-radius: 30px;
  height: 30px;
  width: 30px;
  background-color: white;
  margin: 10px;
  padding: 10px;
}

.rondFlex {
  display: flex;
  flex-wrap: wrap;
}

.canard1 {
  background-image: url("../assets/img/canard1.jpeg");
  background-repeat: round;
}
.canard2 {
  background-image: url("../assets/img/canard2.jpeg");
  background-repeat: round;
}
.canard3 {
  background-image: url("../assets/img/canard3.jpeg");
  background-repeat: round;
}
.canard4 {
  background-image: url("../assets/img/canard4.jpeg");
  background-repeat: round;
}
.canard5 {
  background-image: url("../assets/img/canard5.jpeg");
  background-repeat: round;
}

.canard6 {
  background-image: url("../assets/img/canard6.jpg");
  background-repeat: round;
}
.plus {
  color: grey;
  text-align: center;
  font-size: 24px;
  cursor: pointer;
}
</style>
