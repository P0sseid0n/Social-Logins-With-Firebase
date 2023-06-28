<script setup lang="ts">
import { ref } from 'vue'
import firebaseConfig from "./FirebaseConfig"
import { getAuth, signInWithPopup, TwitterAuthProvider, GoogleAuthProvider, FacebookAuthProvider, GithubAuthProvider, type User, signOut } from "firebase/auth"
import { initializeApp } from 'firebase/app'


initializeApp(firebaseConfig)

const auth = getAuth()

const user = ref<User>()

async function handleSignin(provider: TwitterAuthProvider | GoogleAuthProvider | FacebookAuthProvider | GithubAuthProvider) {
   try {
      const result = await signInWithPopup(auth, provider)
      user.value = result.user
   } catch (error) {
      console.error(error)
   }
}


function handleLogout() {
   signOut(auth).then(() => {
      user.value = undefined
   }).catch((error) => {
      console.error(error)
   })
}

</script>

<template>
   <h1>Social Logins</h1>
   <div v-if="user">
      <img v-if="user.photoURL" :src="user.photoURL" :alt="user.displayName || 'UserPhoto'" class="avatar">
      <h3>
         {{ user.displayName }}
      </h3>
      <h4>
         {{ user.email }}
      </h4>
   </div>
   <div v-if="user" id="LogInButtons">

      <button @click="handleLogout" class="logoutButton">LogOut</button>
   </div>
   <div v-else id="LogInButtons">
      <h5>Login with... </h5>
      <button @click="() => handleSignin(new GoogleAuthProvider())" class="googleButton">
         <img src="./assets/Google.png" alt="">
         <span>Google</span>
      </button>
      <!-- <button @click="" class="appleButton">
         <img src="./assets/Apple.png" alt="">
         <span>Apple</span>
      </button> -->
      <button @click="() => handleSignin(new FacebookAuthProvider())" class="facebookButton">
         <img src="./assets/Facebook.png" alt="">
         <span>Facebook</span>
      </button>
      <button @click="() => handleSignin(new TwitterAuthProvider())" class="twitterButton">
         <img src="./assets/Twitter.png" alt="">
         <span>Twitter</span></button>
      <button @click="() => handleSignin(new GithubAuthProvider())" class="githubButton">
         <img src="./assets/Github.png" alt="">
         <span>Github</span>
      </button>

   </div>
</template>

<style>
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;700&display=swap');

* {
   margin: 0;
   padding: 0;
   box-sizing: border-box;
   font-family: 'Inter', sans-serif;
   font-weight: 500;
}

body {
   color: black;
   background: rgb(247, 247, 253);
}

#app {
   display: flex;
   justify-content: center;
   align-items: center;
   height: 100vh;
   flex-direction: column;
}

h1 {
   text-align: center;
   display: block;
   margin-bottom: 32px;
}

#app>div {
   display: flex;
   flex-direction: column;
   justify-content: center;
   align-items: center;
}

#app #LogInButtons {
   width: 100%;
   max-width: 196px;
}

img.avatar {
   width: 96px;
   border-radius: 50%;
   margin-bottom: 16px;
}

h3 {
   color: rgb(35, 35, 35);
}

h4,
h5 {
   text-align: center;
   margin-bottom: 8px;
   color: rgb(95, 95, 95);
   font-weight: 500;
}

h5 {

   font-size: 16px;
}

button {
   color: white;
   border-radius: 8px;
   font-weight: bold;
   font-size: 16px;
   cursor: pointer;
   border: none;

   display: block;

   height: 48px;
   width: 100%;
   max-width: 196px;

   margin-bottom: 16px;

   display: flex;
   justify-content: center;
   align-items: center;
   gap: 8px;
}

.logoutButton {
   background-color: #ed1b24;
   margin-top: 32px;
}

.googleButton {
   background-color: #4285f4;
}

.appleButton {
   background-color: #000000;
}

.facebookButton {
   background-color: #1877f2;
}

.twitterButton {
   background-color: #1D9BF0;
}

.githubButton {
   background-color: #24292f;
}
</style>
