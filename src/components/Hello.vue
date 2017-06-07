<template>
  <div class="container">
    <div class="page-header">
      <h1>Vue.js 2 & Firebase sample application</h1>

    </div>
    <div class="panel panel-default">
      <div class="panel-heading">
        <h3>Add a Book</h3>

      </div>

      <div class="panel-body">
        <form class="form-inline" id="form" v-on:submit.prevent="addBook">
            <div class="form-group">
              <label for="bookTitle">Title: </label>
              <input type="text" id="bookTitle" v-model="newBook.title" class="form-control" />
            </div>
            <div class="form-group">
              <label for="author">Author: </label>
              <input type="text" id="author" v-model="newBook.author" class="form-control" />
            </div>
            <div class="form-group">
              <label for="url">Url: </label>
              <input type="text" id="url" v-model="newBook.url" class="form-control" />
            </div>

            <input type="submit" class="btn btn-primary" value="Add Book">

        </form>
      </div>
    </div>

    <div class="panel panel-default">
      <div class="panel-heading">
        <h3>Books Lists</h3>

      </div>

      <div class="panel-body">
        <table class="table table-striped">
          <thead>
            <tr>
              <th>Title</th>
              <th>
                Author
              </th>
              <th>
                Delete
              </th>

            </tr>
          </thead>

          <tbody>
            <tr v-for="book in books">
              <td><a v-bind:href="book.url">{{book.title}}</a></td>
              <td>{{book.author}}</td>
              <td>
                <span class="glyphicon glyphicon-trash" v-on:click="removeBook(book)"></span>
              </td>

            </tr>
          </tbody>

        </table>
      </div>

    </div>

  </div>
</template>

<script>
import Firebase from 'firebase'
import toastr from 'toastr'
let config = {
  apiKey: 'AIzaSyDAlLsOJ9JQsnMUYA7kvC4n0amcdTASe24',
  authDomain: 'fire2-ba223.firebaseapp.com',
  databaseURL: 'https://fire2-ba223.firebaseio.com',
  projectId: 'fire2-ba223',
  storageBucket: 'fire2-ba223.appspot.com',
  messagingSenderId: '853308912735'
}
let app = Firebase.initializeApp(config)
let db = app.database()
let bookRef = db.ref('books')
export default {
  name: 'hello',
  firebase: {
    books: bookRef
  },
  data () {
    return {
      newBook: {
        title: '',
        author: '',
        url: ''
      }
    }
  },
  methods: {
    addBook: function () {
      bookRef.push(this.newBook)
      this.newBook.title = ''
      this.newBook.author = ''
      this.newBook.url = ''
      toastr.success('Book Added')
    },
    removeBook: function (book) {
      bookRef.child(book['.key']).remove()
      toastr.success('Book removed')
    }
  }
}

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
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
