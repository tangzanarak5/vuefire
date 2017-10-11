<template>
  <div id="app">
    <div v-for="post in posts">
      {{post.text}}<button type="button" name="button" @click="removePost(post)">X</button>
    </div>

          <div>
            <textarea cols="" v-model="text" @keydown.enter="addPost"></textarea>
          </div>

          <div>
            <button type="button" name="button" @click="addPost">Post</button><br>
          </div>

          <br><br><br>

          <div id="app-6">
  Name: <input type="text" v-model="inputName"><br>
  Email: <input type="text" v-model="inputEmail"><br>
  <button @click="addUser">Add</button>
</div>

<div v-for="user, index in users">
  {{ user.name }}
  {{ user.email }}
  <button @click="deleteUser(index)">Delete</button>
</div>

        </div>

    <!-- <router-view></router-view> -->
</template>

<script>
import firebase from 'firebase'
var config = {
  apiKey: 'AIzaSyDpAk__hxVCgB7W13Y1kdNLmFWYMJg_874',
  authDomain: 'testvuefirena.firebaseapp.com',
  databaseURL: 'https://testvuefirena.firebaseio.com',
  projectId: 'testvuefirena',
  storageBucket: '',
  messagingSenderId: '1059398698064'
}
var testvuefirena = firebase.initializeApp(config)
var db = testvuefirena.database()
export default {
  name: 'app',
  data () {
    return {
      posts: [],
      text: '',
      inputName: '',
      inputEmail: '',
      users: [
        {
          name: 'nati',
          email: 'na@sellsuki.com'
        },
        {
          name: 'somchai',
          email: 'somchai@kmutnb.ac.th'
        }
      ]
    }
  },
  mounted () {
    var vm = this
   // ผูก database เข้ากับตัวแปร posts แบบ Array  AsObject
    vm.$bindAsArray('posts', db.ref('posts'))
    vm.$bindAsArray('test', db.ref('test'))
  },
  methods: {
    addPost () {
      // เพิ่มข้อมูล
      var vm = this
      this.$firebaseRefs.posts.push({
        text: vm.text
      })
      this.$firebaseRefs.test.push({
        text: vm.text
      })
      vm.text = ''
    },
    removePost (post) {
      // ลบข้อมูล
      console.log(post['.key'])
      this.$firebaseRefs.posts.child(post['.key']).remove()
    },
    addUser () {
      if (this.inputName === '' || this.inputEmail === '') {
        return
      }
      this.users.push({
        name: this.inputName,
        email: this.inputEmail
      })
      this.inputName = ''
      this.inputEmail = ''
    },
    deleteUser (index) {
      console.log(index)
      this.users.splice(index, 1)
    }
  }
}
</script>

<style>
.box {
  /*border: 1px black solid;*/
}
.posts {
  height: 70vh;
  border: 1px black solid;
  border-radius: 20px;
  overflow: auto;
}
.post{
  margin-top: 5%;
}
.display-photo {
  width: 100%;
}
.input-text {
  width: 100%;
}
</style>
