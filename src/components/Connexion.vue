<template>
  <div>
    <!-- formulaire de connexion -->
    <section id="logging">
      <div class="logoBocalac">
        <img
          src="../assets/img/canardBocalacContours.png"
          alt="canardBocalacContours"
        />
      </div>

      <div v-show="!show" class="logForm">
        <form @submit.prevent="login" class="form">
          <div class="logoTextBocalac">
            <img
              src="../assets/img/bocalacTextWhite.png"
              alt="bocalacTextBlack"
            />
          </div>

          <label class="alignPseudo" for="pseudo">Email </label> <br />
          <input type="email" id="pseudo" v-model="email" required />
          <br />

          <div class="alignMdpForgetMdp">
            <label class="labelMdp" for="mdp">Mot de passe </label>
            <a
              href="https://www.sebastien-martinez.com/problemes-de-memoire/"
              target="_blank"
              >mot de passe oublié?
            </a>
          </div>
          <input type="password" name="mdp" id="mdp" v-model="mdp" required />
          <br />

          <button type="submit">VALIDER</button>
          <br />

          <div class="linkInscription">
            <p class="animationHover" @click="show = !show">
              Tu n'as pas de compte? Inscris-toi.
            </p>
          </div>

          <p v-if="result === true" class="success">
            Connexion réussie
            <br />
            <span v-show="false"> Token: {{ token }}</span>
          </p>
          <p v-else-if="result === false" class="error">Connexion échouée</p>
        </form>
      </div>

      <!-- formulaire d'inscription -->
      <section id="inscription">
        <div v-show="show" class="logForm">
          <form @submit.prevent="inscription" class="formInscription">
            <div class="logoTextBocalac">
              <img
                src="../assets/img/bocalacTextWhite.png"
                alt="bocalacTextBlack"
              />
            </div>

            <div class="backToConnexion">
              <p @click="show = !show">CTRL+Z</p>
            </div>

            <label class="mailStyle" for="mail">Mail </label> <br />
            <input type="email" name="mail" id="mail" v-model="mail" required />

            <p
              class="textVerification"
              v-show="!isEmailValid && this.mail.length >= 1"
            >
              Veuillez rentrer un email valide.
            </p>

            <label class="pseudoInscription" for="pseudoInscription"
              >Nom
            </label>
            <br />
            <input
              type="text"
              id="nomInscription"
              v-model="nomInscription"
              :class="isLastNameValid"
              required
            />

            <br />

            <label class="pseudoInscription" for="pseudoInscription"
              >Prénom
            </label>
            <br />
            <input
              type="text"
              id="prenomInscription"
              v-model="prenomInscription"
              :class="isFirstNameValid"
              required
            />

            <br />

            <label class="mdpInscription" for="createMdp">Mot de passe </label>
            <br />
            <input
              type="password"
              name="createMdp"
              id="createMdp"
              v-model="createMdp"
              required
            />
            <p
              class="textVerification"
              v-show="!isPasswordValid && this.createMdp.length >= 1"
            >
              Un mot de passe doit faire au moins 8 caractères dont une lettre
              majuscule et un chiffre.
            </p>
            <br />

            <label class="confirmMdpStyle" for="confirmMdp"
              >Confirmation du mot de passe
            </label>
            <br />
            <input
              type="password"
              name="confirmMdp"
              id="confirmMdp"
              v-model="confirmMdp"
              :class="verifyPassword"
            />
            <p
              class="textVerification"
              v-show="verifyPassword == false && this.confirmMdp.length >= 1"
            >
              Veuillez entrer le même mot de passe.
            </p>
            <p class="textVerificationGreen" v-show="verifyPassword == true">
              Mot de passe identique
            </p>

            <br />

            <button
              v-show="
                isEmailValid &&
                isLastNameValid &&
                isFirstNameValid &&
                isPasswordValid &&
                verifyPassword
              "
              type="submit"
              @click="show = !show"
            >
              VALIDER
            </button>
            <br />
          </form>
        </div>
      </section>
    </section>
  </div>
</template>

<script>
export default {
  data() {
    return {
      show: false,
      // connexion data
      email: "",
      mdp: "",
      result: null,
      token: "",
      // inscription data
      mail: "",
      nomInscription: "",
      prenomInscription: "",
      createMdp: "",
      confirmMdp: "",
    };
  },

  // inscription
  methods: {
    async inscription() {
      const options = {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify({
          email: this.mail,
          lastname: this.nomInscription,
          firstname: this.prenomInscription,
          password: this.createMdp,
        }),
      };

      const response = await fetch(
        "https://social-network-api.osc-fr1.scalingo.io/bocalac/register",
        options
      );

      const data = await response.json();

      console.log(data);

      this.mail = "";
      this.nomInscription = "";
      this.prenomInscription = "";
      this.createMdp = "";
      this.confirmMdp = "";
    },

    // Connexion
    async login() {
      const options = {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify({
          email: this.email,
          password: this.mdp,
        }),
      };

      const response = await fetch(
        "https://social-network-api.osc-fr1.scalingo.io/bocalac/login",
        options
      );

      const data = await response.json();

      this.result = data.success;
      if (data.success === true) {
        this.token = data.token;
        localStorage.setItem("token", this.token);
      }

      // Navigation vers une autres page avec router.push
      this.$router.replace("/actus");
      this.email = "";
      this.mdp = "";
    },
  },

  computed: {
    isEmailValid() {
      const regex = new RegExp(/\b[\w\.-]+@[\w\.-]+\.\w{2,4}\b/);
      return regex.test(this.mail);
    },

    isFirstNameValid() {
      const length = this.nomInscription.length;
      return length >= 1;
    },

    isLastNameValid() {
      const length = this.prenomInscription.length;
      return length >= 1;
    },

    isPasswordValid() {
      const regex = new RegExp(/(?=.*\d)(?=.*[A-Z]).{8,}/);
      return regex.test(this.createMdp);
    },

    verifyPassword() {
      const mdp = this.createMdp;
      const verifMdp = this.confirmMdp;
      if (mdp != verifMdp) {
        return false;
      } else if (mdp === "") {
        return false;
      } else {
        return true;
      }
    },
  },
};
</script>

<style scoped>
/* formulaire de connexion */
#logging {
  display: flex;
  min-height: 96vh;
  justify-content: center;
  align-items: center;
  gap: 5%;
  background: white;
  position: absolute;
  z-index: 1000;
  top: 0;
  left: 0;
  right: 0;
  height: 100vh;
}
.logoBocalac img {
  width: 300px;
  height: 300px;
}

.logoTextBocalac img {
  width: 230px;
  height: 40px;
  padding-top: 40px;
  padding-bottom: 40px;
}

.form {
  border: solid 1px white;
  text-align: center;
  width: 420px;
  border-radius: 10px;
  background-color: #474e58;
  border: 2px solid white;
  box-shadow: 3px 6px 6px grey;
}

#pseudo {
  width: 300px;
  margin-bottom: 40px;
  border-radius: 10px;
  padding-top: 5px;
  padding-bottom: 5px;
  border: 2px solid white;
  outline: #5adfbc;
}

.alignPseudo {
  margin-right: 252px;
  color: white;
}

#mdp {
  width: 300px;
  margin-bottom: 40px;
  border-radius: 10px;
  padding-top: 5px;
  padding-bottom: 5px;
  border: 2px solid white;
  outline: #5adfbc;
}

.alignMdpForgetMdp {
  padding-bottom: 2px;
}

.labelMdp {
  padding-right: 80px;
  color: white;
}

.alignMdpForgetMdp a {
  color: white;
  font-size: 15px;
  font-weight: 200;
  text-decoration: none;
  font-style: italic;
}

.alignMdpForgetMdp a:hover {
  color: #5adfbc;
  font-size: 15px;
  font-weight: 200;
  text-decoration: none;
  font-style: italic;
}

.logForm button {
  padding: 10px 25px;
  margin-bottom: 40px;
  background-color: white;
  border-radius: 10px;
  color: grey;
  border: 2px solid #5adfbc;
  outline: none;
  cursor: pointer;
  font-weight: bold;
  letter-spacing: 6px;
}

.logForm button:hover {
  padding: 10px 25px;
  margin-bottom: 40px;
  background-color: #5adfbc;
  border-radius: 10px;
  color: white;
  border: 2px solid #5adfbc;
  outline: none;
  cursor: pointer;
}

/* Hover sur  "inscris toi" paragraphe */

.animationHover {
  box-shadow: inset 0 0 0 0 #5adfbc;
  color: #5adfbc;
  padding: 0 0.25rem;
  margin: 0 -0.25rem;
  transition: color 0.3s ease-in-out, box-shadow 0.3s ease-in-out;
  border-radius: 10px;
  cursor: pointer;
}
.animationHover:hover {
  color: #474e58;
  box-shadow: inset 400px 0 0 0 #5adfbc;
  border-radius: 10px;
}

.animationHover {
  color: #5adfbc;
  font-size: 17px;
  line-height: 1.5;
  font-weight: 700;
  text-decoration: none;
  text-align: initial;
  margin-left: 88px;
  margin-right: 72px;
}

.error {
  color: red;
  font-size: 20px;
}

.success {
  color: #5adfbc;
  font-size: 20px;
}

/* style formulaired d'inscription */

.formInscription {
  border: solid 1px white;
  text-align: center;
  width: 420px;
  height: 670px;
  border-radius: 10px;
  background-color: #474e58;
  border: 2px solid white;
  box-shadow: 3px 6px 6px grey;
}

.linkInscription {
  margin-bottom: 40px;
}

.linkInscription p {
  color: #5adfbc;
}

/* animation hover ctrl+z */

.backToConnexion {
  box-shadow: inset 0 0 0 0 #5adfbc;
  color: #5adfbc;
  padding: 0 0.25rem;
  margin: 0 -0.25rem;
  transition: color 0.3s ease-in-out, box-shadow 0.3s ease-in-out;
  width: 90px;
}
.backToConnexion:hover {
  color: #474e58;
  box-shadow: inset 95px 0 0 0 #5adfbc;
}

.backToConnexion {
  color: #5adfbc;
  font-size: 20px;
  line-height: 1.5;
  font-weight: 700;
  text-decoration: none;
  text-align: initial;
  margin-left: 160px;
  margin-bottom: 30px;
  margin-top: 0px;
  letter-spacing: 1px;
}
.backToConnexion p {
  margin-top: 0px;
  cursor: pointer;
}

/* fin animation */
.mailStyle {
  color: white;
  margin-right: 17rem;
}

.pseudoInscription {
  color: white;
  margin-right: 16rem;
}

.mdpInscription {
  color: white;
  margin-right: 13.5rem;
}

#mail {
  width: 300px;
  margin-bottom: 20px;
  border-radius: 10px;
  border: 2px solid white;
  padding-bottom: 5px;
  padding-top: 5px;
  outline: none;
}

#nomInscription {
  width: 300px;
  margin-bottom: 20px;
  border-radius: 10px;
  border: 2px solid white;
  padding-bottom: 5px;
  padding-top: 5px;
  outline: none;
}

#prenomInscription {
  width: 300px;
  margin-bottom: 20px;
  border-radius: 10px;
  border: 2px solid white;
  padding-bottom: 5px;
  padding-top: 5px;
  outline: none;
}

#createMdp {
  width: 300px;
  margin-bottom: 20px;
  border-radius: 10px;
  border: 2px solid white;
  padding-bottom: 5px;
  padding-top: 5px;
  outline: none;
}

#confirmMdp {
  width: 300px;
  margin-bottom: 20px;
  border-radius: 10px;
  border: 2px solid white;
  padding-bottom: 5px;
  padding-top: 5px;
  outline: none;
}

.confirmMdpStyle {
  color: white;
  margin-right: 7rem;
}

.textVerification {
  color: white;
  margin-left: 50px;
  margin-right: 50px;
}

.textVerificationGreen {
  color: #5adfbc;
}
</style>
