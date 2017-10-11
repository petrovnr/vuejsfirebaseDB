<template>
<div id="app">

  <div class="container">

    <form class="form" v-on:sumbit.prevent="addBook">
      <fieldset>

        <!-- Form Name -->
        <h3>Добавить новую книгу</h3>

        <!-- Text input-->
        <div class="form-group">
          <label class="col-md-12 control-label" for="textinput">Название:</label>
          <div class="col-md-12">
            <input type="text" id="bookTitle" class="form-control" v-model="newBook.title">

          </div>
        </div>

        <!-- Text input-->
        <div class="form-group">
          <label class="col-md-12 control-label" for="textinput">Автор:</label>
          <div class="col-md-12">
            <input type="text" id="bookAuthor" class="form-control" v-model="newBook.author">

          </div>
        </div>

        <!-- Text input-->
        <div class="form-group">
          <label class="col-md-12 control-label" for="textinput">url: http://domain.zone</label>
          <div class="col-md-12">
            <input type="text" id="bookUrl" class="form-control" v-model="newBook.url">

          </div>
        </div>

        <!-- Button -->
        <div class="form-group">
          <label class="col-md-12 control-label" for="singlebutton"></label>
          <div class="col-md-12">
            <button class="btn btn-primary pull-center" @click.prevent="addBook">Добавить</button>
          </div>
        </div>

      </fieldset>
    </form>

    <div class="panel panel-default">
      <div class="panel-heading">
        <h3>Список книг</h3>
      </div>
      <div class="panel-body">
        <table class="table table-striped">
          <thead>
            <tr>
              <th>
                Название
              </th>
              <th>
                Автор
              </th>
              <th>
                Действие
              </th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="book in books">
              <td>
                <a target="_blank" v-bind:href="book.url">{{book.title}}</a>
              </td>
              <td>
                {{book.author}}
              </td>
              <td>
                <span class="badge badge-pill badge-danger" @click="removeBook(book)">Удалить</span>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>

  </div>
</div>
</template>

<script>
import Hello from './components/Hello'

import toastr from 'toastr'

import Firebase from 'firebase' // Изициализировали объект

let config = {
  apiKey: "AIzaSyCovL0QsHsgHXCoPU5Irw26qjjoso5H5Tg",
  authDomain: "booklist-fe599.firebaseapp.com",
  databaseURL: "https://booklist-fe599.firebaseio.com",
  storageBucket: "booklist-fe599.appspot.com",
  messagingSenderId: "37507968873"
} // Залогинились

let app = Firebase.initializeApp(config); // API логин
let db = app.database(); // АРI метод работы с БД
let booksRef = db.ref('books');

export default {
  name: 'app',
  firebase: {
    books: booksRef
  },
  data() {
    return {
      newBook: {
        title: '',
        author: '',
        url: ''
      }
    }
  },
  methods: {
    addBook: function() {
      booksRef.push(this.newBook);
      this.newBook.title = '';
      this.newBook.author = '';
      this.newBook.url = '';
    },
    removeBook: function(book) {
      booksRef.child(book['.key']).remove();
      toastr.success("Книга успешно удалена!");
    }
  }

}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
