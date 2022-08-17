<template>
  <div class="hello container-fluid">
    <div v-show="page1">
      <input type="text" placeholder="Search for a book..." v-model="search" @keyup.enter="searching()">

    <br>
    <div v-show="this.search.length" class="row first-row">
      <h1>Search Item for {{this.search}}</h1><br><br>
    </div>
    <!--<div>
      <ul>
        <li v-for="task in tasks" :key="task.id">{{task.body}}</li>
      </ul>
    </div>

    <ul>
      <li v-for="(prod, index) in prods" :key="index">{{prod.name}}</li>
    </ul>-->



    <ul class="row second-row"  >
      <li v-for="(item, index) in items" v-bind:key="index" class="col-xs-6 col-md-2" >
        <span @click="page1=!page1; view(index)">
          <img :src="item.volumeInfo.imageLinks.thumbnail" class="img-poster">
          <h2>{{item.volumeInfo.title}}</h2>
        </span>
      </li>
    </ul>


    <!--<ul >
      <li v-for="item in items" v-bind:key="item.id">
        <h2>{{item.volumeInfo.title}}</h2>
      </li>
    </ul>-->


    </div>

    <div class="row third-row" v-show="!page1">
      <div class="row" id="top">
        <div class="col-md-12"><button class="back-btn" @click="page1 = !page1">Back</button></div>
      </div>
      <div class="row "  v-for="(book, index) in books" v-bind:key="index">


        <div class="col-md-6"><img :src="book.volumeInfo.imageLinks.thumbnail" class="img-view"></div>
        <div class="col-md-6 content">
          <h4 class="text2">Title: {{book.volumeInfo.title}}</h4>
          <h4 class="text2">Description: {{book.volumeInfo.description}}</h4>
          <h4 class="text2">Published Date: {{book.volumeInfo.publishedDate}}</h4>
          <h4 class="text2">Ratings: {{book.volumeInfo.ratingsCount}}</h4>
          <h4 class="text2">Average Ratings: {{book.volumeInfo.averageRating}}</h4>
          <h4 class="text2">Maturity Rating: {{book.volumeInfo.maturityRatin}}</h4>
          <h4 class="text2">Categories: {{book.volumeInfo.categories}}</h4>
          <h4 class="text2">Pagination: {{book.volumeInfo.pageCount}}</h4>
          <h4 class="text2">Content Version: {{book.volumeInfo.contentVersion}}</h4>
          <h4 class="text2">Author: {{book.volumeInfo.authors}}</h4>
          <h4 class="text2">Print Type: {{book.volumeInfo.printType}}</h4>
          <!--<h4 class="text2">Content Version: {{book.volumeInfo.contentVersion}}</h4>-->
          <h4 class="text2">Language: {{book.volumeInfo.language}}</h4>
          <h4 class="text2">Preview: <a :href="book.volumeInfo.infoLink"><button class="btn btn-danger">click</button></a></h4>

        </div>
      <!--<div class="col-md-6" >
        <div v-for="(book, index) in books" v-bind:key="index">
          <img :src="book.volumeInfo.imageLinks.thumbnail" class="img-view">
          <h4 class="text">{{book.volumeInfo.title}}</h4>
        </div>
      </div>
      <div class="col-md-6">
        <h3 v-for="(book, index) in books" v-bind:key="index">{{book.volumeInfo.title}}</h3>
      </div>-->
      </div>

      <div class="row">
        <h1><b>Recommended for you</b></h1>
        <ul class="row second-row"  >
          <li v-for="(category, index) in categories" v-bind:key="index" class="col-xs-6 col-md-2" >
            <a href="#top">
              <span @click="view2(index)">
              <img :src="category.volumeInfo.imageLinks.thumbnail" class="img-poster">
              <h2>{{category.volumeInfo.title}}</h2>
              </span>
            </a>
          </li>
        </ul>
      </div>
    </div>

  </div>

















</template>

<script>
import axios from 'axios'

export default {
  name: 'HelloWorld',
  data() {
    return {
      search: "",
      page1: true,
      selectedId: null,
      selectedCategory: null,
      tasks: [],
      prods: [
        {name: '7y7ygygy'},
        {name: '7tftfty'}
      ],
      books: {},
      categories: {},
      items: {}
    }
  },
  methods: {
    searching() {
      //axios.get('https://api.themoviedb.org/3/keyword/naruto?api_key=1ecbee71bda061d7bd8aa8bed6700aca')
      axios.get('https://www.googleapis.com/books/v1/volumes?q='+ this.search + '&maxResult=30')
      .then((response) => {
        console.log(response.data.items)
        this.items = response.data.items
      } )
      console.log(this.search)
    },
    view(index) {
      this.selectedId = this.items[index].id;
      axios.get('https://www.googleapis.com/books/v1/volumes?q='+ this.selectedId )
      .then((response) => {
        console.log(response.data.items)
        this.books = response.data.items
      } ),

//this is for the recommended api
      this.selectedCategory = this.items[index].volumeInfo.categories;
      axios.get('https://www.googleapis.com/books/v1/volumes?q='+ this.selectedCategory )
      .then((response) => {
        console.log(response.data.items)
        this.categories = response.data.items
      } )

    },

    view2(index) {
      this.selectedId = this.categories[index].id;
      axios.get('https://www.googleapis.com/books/v1/volumes?q='+ this.selectedId )
      .then((response) => {
        console.log(response.data.items)
        this.books = response.data.items
      } ),

//this is for the recommended api
      this.selectedCategory = this.categories[index].volumeInfo.categories;
      axios.get('https://www.googleapis.com/books/v1/volumes?q='+ this.selectedCategory )
      .then((response) => {
        console.log(response.data.items)
        this.categories = response.data.items
      } )

    }
  },
  async mounted() {
    //axios.get('https://jsonplaceholder.typicode.com/comments')
    axios.get('https://www.googleapis.com/books/v1/volumes?q=book&maxResults=40')
      .then((response) => {
        console.log(response.data.items)
        this.items = response.data.items
        //alert(this.tasks.name)
      } )



  }
}

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
@import url('http://fonts.cdnfonts.com/css/euclid-circular-a');
* {
  margin: 0px;
  padding: 0px;
  box-sizing: border-box;
  overflow-x: hidden;
}
.hello {
  color: #fff;
}

a {
  color: #e70a0a;
}

input[type=text]{
  margin-top: 50px;
  width: 500px;
  background: #2e2e2e;
  height: 50px;
  border: none;
  border-radius: 15px;
  padding: 0px 10px 0px 13px;
  color: #fff;
  font-size: 20px;
  transition: all 0.4s ease;
}
input[type=text]:focus {
  padding: 23px;
  transform: scale(1.02);
}
li {
  list-style: none;
}

.first-row h1 {
  color: red;
  margin-top: 30px;
}

.second-row {
  margin-top: 40px;
  text-align: center;
}
.second-row .img-poster {
  /*transform: scale(1.3);*/
  height: 340px;
  /*width: 250px;*/
  /*margin-bottom: 100px;*/
  /*margin-top: 20px;*/
  margin-left: 0px;
  margin-right: 0px;
  margin-bottom: 20px;
}

.second-row section {
  /*height: 340px;
  width: 270px;*/
  background-color: #2e2e2e;
  margin-bottom: 100px;
  /*margin-left: 30px;
  margin-right: 30px;*/

  border: 1px solid #474747;
  border-radius: 5px;

  transition: all 0.2s ease;
}

span h2, .text {
  font-size: 16px;
  margin: 20px;
  font-family: 'Euclid Circular A';
  margin-top: -15px;
}
.text2 {
  font-size: 16px;
  /*margin: 20px;*/
  font-family: 'Euclid Circular A';
  text-align: left;
}
section h2 {
  font-size: 16px;
  margin: 20px;
}
section:hover {
  transform: scale(1.05);

}
.img-view {
  height: 340px;
  width: 250px;

  margin-left: 0px;
  margin-right: 0px;
  margin-bottom: 20px;
}
.details {
  text-align: left;
}
.col-md-2 {
  /*border: 3px solid green;*/
  padding: 0%;
  /*margin: 5px;*/

  /*transform: scale(0.9);*/
}
/*.col-md-9 {
  border: 3px solid blue;
}*/
.back-btn {
  position: relative;
  display: block;
  background: #e70a0a;
  color: #fff;
  /*width: 50px;*/
  padding: 5px;
  padding-left: 15px;
  padding-right: 15px;
  border-radius: 7px;
  border: none;
  font-family: 'Euclid Circular A';
  margin-bottom: 20px;
  float: left;
}

.content {
  margin-bottom: 40px;
  border-bottom: 1px solid #474747;
  padding: 10px;
}

@media screen and (max-width: 425px){
  .hello {
    overflow-x: hidden;
    color: #fff;
  }

  input[type=text]{
    width: 350px;
    background: #2e2e2e;
    height: 40px;
  }
  .second-row section {
    background-color: #0f1011;
    border: 1px solid #0f1011;
    border-bottom: 1px solid #474747;
  }

}

@media screen and (min-width: 576px) and (max-width: 768px) {
  .hello {
    overflow-x: hidden;
    color: #fff;
  }

  input[type=text]{
    width: 350px;
    background: #2e2e2e;
    height: 40px;
  }
  .second-row section {
    background-color: #0f1011;
    border: 1px solid #0f1011;
    border-bottom: 1px solid #474747;
  }
  .second-row .img-poster {
    /*transform: scale(1.3);*/
    height: 100%;
    width: 100%;
    /*margin-bottom: 100px;*/
    /*margin-top: 20px;*/
    margin-left: 0px;
    margin-right: 0px;
    margin-bottom: 20px;
  }

}


</style>
