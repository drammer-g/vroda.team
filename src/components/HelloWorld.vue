<template>
  <div class="hello">
    <h1 v-if="getUserName">hello {{ getUserName }} </h1>
    <button @click="signIn">Sign in</button>
    <button @click="signOutUser">Exit</button>
    <button @click="saveMessage">Save Message</button>
  </div>
</template>

<script>
/* eslint-disable */
import {initializeApp} from 'firebase/app';
// import {
//   getAuth,
//   onAuthStateChanged,
//   GoogleAuthProvider,
//   signInWithPopup,
//   signOut,
// } from 'firebase/auth';

import {
  getAuth,
  onAuthStateChanged,
  GoogleAuthProvider,
  signInWithPopup,
  signOut,
} from 'firebase/auth';
// import {
//   getFirestore,
//   collection,
//   addDoc,
//   query,
//   orderBy,
//   limit,
//   onSnapshot,
//   setDoc,
//   updateDoc,
//   doc,
//   serverTimestamp,
// } from 'firebase/firestore';
//
import {
  getFirestore,
  collection,
  addDoc,
  serverTimestamp,
} from 'firebase/firestore';

// import {
//   getStorage,
//   ref,
//   uploadBytesResumable,
//   getDownloadURL,
// } from 'firebase/storage';
// import { getMessaging, getToken, onMessage } from 'firebase/messaging';
// import { getPerformance } from 'firebase/performance';

import firebaseConfig from "../../firebase-config";
// Import the functions you need from the SDKs you need
// import { getAnalytics } from "firebase/analytics";
// TODO: Add SDKs for Firebase products that you want to use
// https://firebase.google.com/docs/web/setup#available-libraries

// Initialize Firebase
const app = initializeApp(firebaseConfig);
// const analytics = getAnalytics(app);

export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data() {
    return {
      currentUser: null
    }
  },
  computed: {
    // Returns the signed-in user's display name.
    getUserName: {
      get() {
        return (!!getAuth()?.currentUser) ? this.currentUser.displayName : null;
      }
    },
  },
  methods: {
    // Initialize firebase auth
    initFirebaseAuth() {
      // Listen to auth state changes.
      onAuthStateChanged(getAuth(), authStateObserver);
    },
    signOutUser() {
      signOut(getAuth());
    },
    isUserSignedIn() {
      return !!getAuth().currentUser;
    },
// Signs-in Friendly Chat.
    async signIn() {
      const app = this;
      // Sign in Firebase using popup auth and Google as the identity provider.
      let provider = new GoogleAuthProvider();
      await signInWithPopup(getAuth(), provider).then((result) => {
        app.currentUser = result.user;
      })
    },
    // Saves a new message to Cloud Firestore.
    async saveMessage(messageText) {
      // Add a new message entry to the Firebase database.
      console.log({
        name: this.getUserName,
        text: "messageText test",
        profilePicUrl: null,
        timestamp: serverTimestamp()
      })
      try {
        await addDoc(collection(getFirestore(), 'messages'), {
          name: this.getUserName,
          text: "messageText test",
          profilePicUrl: null,
          timestamp: serverTimestamp()
        });
      } catch (error) {
        console.error('Error writing new message to Firebase Database', error);
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
