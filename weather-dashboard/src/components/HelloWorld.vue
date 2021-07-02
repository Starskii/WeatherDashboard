<template>
  <div class="hello">
    <h1>Weather Dash</h1>
    <ol>
      <li v-for="(z, pos) in temperatureLogs" :key="pos">Time: {{temperatureLogs[pos].datetime}} Temperature: {{temperatureLogs[pos].fahrenheit}}</li>
    </ol>
  </div>
</template>

<script lang="ts">

interface TempData{
  celsius : string,
  datetime : string, 
  fahrenheit : string,
  name : string
}

import { Options, Vue } from 'vue-class-component';
import firebase from 'firebase';
import { FirebaseFirestore, QueryDocumentSnapshot, QuerySnapshot } from "@firebase/firestore-types"


const firebaseConfig = {
    apiKey: "AIzaSyBCkk2SgHALN4YDmEiEHD8KcOocod4vW5Q",
    authDomain: "temperaturelogger-a3bf4.firebaseapp.com",
    databaseURL: "https://temperaturelogger-a3bf4-default-rtdb.firebaseio.com",
    projectId: "temperaturelogger-a3bf4",
    storageBucket: "temperaturelogger-a3bf4.appspot.com",
    messagingSenderId: "196397857924",
    appId: "1:196397857924:web:4ffd6c2350170fa95d96d1",
    measurementId: "G-P17DD4RE25"
  };

const firebaseApp = firebase.initializeApp(firebaseConfig);
const appDB = firebase.firestore();

export default class HelloWorld extends Vue {

  temperatureLogs : TempData[] = [];

    mounted(): void{
    appDB.
    collection("/temperature-logs")
    .orderBy("datetime")
    .onSnapshot((qs: QuerySnapshot): void => {
      this.temperatureLogs.splice(0);
      qs.forEach((qds: QueryDocumentSnapshot): void => {
        if(qds.exists){
          const data = qds.data();
          this.temperatureLogs.push(data as TempData);
        }
      })
    });
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
