<template>
  <div class="body">
    <HeaderBar class="headerBar" :nom="lastname" :prenom="firstname" />
    <AsideBar class="aside" />
    <router-view
      :nom="lastname"
      :prenom="firstname"
      :mail="email"
      :methodGetUser="getUser"
    />
  </div>
</template>

<script>
import HeaderBar from "@/components/HeaderBar.vue";
import AsideBar from "@/components/AsideBar.vue";

const App = {
  components: {
    HeaderBar,
    AsideBar,
  },

  data() {
    return {
      firstname: "",
      lastname: "",
      email: "",
    };
  },

  mounted() {
    this.getUser();
  },

  methods: {
    async getUser() {
      const options = {
        method: "GET",
        headers: {
          "Content-Type": "application/json",
          Authorization: "bearer " + localStorage.getItem("token"),
        },
      };

      const response = await fetch(
        "https://social-network-api.osc-fr1.scalingo.io/bocalac/user",
        options
      );

      const data = await response.json();

      this.firstname = data.firstname;
      this.lastname = data.lastname;
      this.email = data.email;

      console.log(response);
      console.log(data);
      console.log(this.firstname + this.lastname + this.email);
    },
  },

  provide() {
    return {
      getLastname: () => {
        return this.lastname;
      },
      getFirstname: () => {
        return this.firstname;
      },
    };
  },
};
export default App;
</script>

<style scoped>
.body {
  width: 79%;
  padding: 3%;
}

.headerBar {
  position: fixed;
  top: 0;
  width: 100%;
  left: 0;
  right: 0;
  z-index: 1000;
}

nav {
  margin-top: 7%;
}
.aside {
  position: fixed;
  right: 0;
  height: 100vh;
  top: 64px;
  z-index: 100;
}
</style>
