<template>
  <div id="app" class="container">
    <!-- <app-header :inSearch="searchQuery"></app-header> -->
    <div class="row">
      <h1>Lemon Quotes</h1>
        <div class="search">
          <input type="text" v-model="searchQuery" />
          <span class="icon-search"></span>
        </div>
    </div>
    <!-- add Quote -->
      <div class="add-quote">
        <form class="form-inline" id="form" v-on:submit.prevent="addQuote">
          <div class="row">
            <div class="form-author">
              <label for="bookAuthor">Author  </label>
              <input type="text" id="bookAuthor" class="form-control" v-model="newList.author">
            </div>
            <div class="form-tag">
              <label for="bookAuthor">Tag  </label>
              <input type="text" id="bookAuthor" class="form-control" v-model="newList.tag">
            </div>
          </div>
          <div class="row" style="margin-top: 30px;">
            <div class="form-quote">
             <!--  <label for="bookUrl">Quote: </label> -->
              <textarea name="quote" id="" rows="10" v-model="newList.quote" class=""></textarea>
              <input type="submit" class="btn-add-quote" value="Add Quote" />
            </div>
          </div>
        </form>
      </div>
    <!-- Quotes List -->
      <div class="item-container row">
        <!-- <div v-for="list in filteredItems">
          <p class="quote">{{ list.quote }}</p>
          <p class="author">{{ list.author }}</p>
        </div> -->
        <div v-for="list in filteredItems" class="item" :id="list.id">
            <p class="quote">{{ list.quote }}</p>
            <p class="author">{{ list.author }}</p>
            <!-- <p>
              <span>{{ list.tag }}</span>
              <span class="glyphicon glyphicon-trash col-md-3" aria-hidden="true" v-on:click="removeQuote(list)"></span>
            </p> -->
            <div class="sns">
              <span class="icon-icon-facebook"></span>
              <span class="icon-icon-twitter"></span>
              <span class="icon-icon-pinterest"></span>
              <span class="icon-icon-kakao"></span>
            </div>
            <div class="like"><span class="icon-icon-like"></span></div>
        </div>
      </div>

  </div>
</template>

<script>

import Firebase from 'firebase';
import Header from './components/Header.vue';
//import toastr from 'toastr'

let config = {
    apiKey: "AIzaSyA2v5btGNaKpUBvMBh-WUglS5UHy816baA",
    authDomain: "lemon-quotes.firebaseapp.com",
    databaseURL: "https://lemon-quotes.firebaseio.com",
    projectId: "lemon-quotes",
    storageBucket: "lemon-quotes.appspot.com",
    messagingSenderId: "914958490140"
  };

let app = Firebase.initializeApp(config);
let db = app.database();

let rootRef = db.ref('lists');

export default {
  name: 'app',

  firebase: {
    lists: rootRef
  },

  data: () => {
    return {
      newList:
        {
          author: '',
          quote: '',
          tag: ''
        },
      searchQuery: ''
    }
  },

  methods: {
    addQuote: function() {
      rootRef.push(this.newList);
      this.newList.author = '';
      this.newList.tag = '';
      this.newList.quote = '';
    },
    removeQuote: function(list) {
      rootRef.child(list['.key']).remove();
      //toastr.success('Book removed successfully');
    }
  },

  computed: {
    sorted: function() {
      return this.lists.sort((a, b) => a.author > b.author ? 1 : -1);
    },
    reverse: function() {
      return this.lists.reverse()
    },
    // filteredItems: function() {
    //   var that = this;
    //   return that.lists.filter(function(list){
    //     return (list.quote.indexOf(that.searchQuery)&&list.author.indexOf(that.searchQuery)&&list.tag.indexOf(that.searchQuery))!== -1;
    //   });
    // },
    filteredItems: function() {
      var listItem = this.lists;
      var searchString = this.searchQuery;

      if(!searchString){
        return listItem.reverse();
      }

      listItem = listItem.filter(function(list){
        if(list.quote.indexOf(searchString) !== -1){
          return list;
        }
      })

      //return listItem.reverse();
      return listItem;

    }
  },

  components: {
    appHeader: Header
  }
}



</script>

<style>
#app {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
</style>
