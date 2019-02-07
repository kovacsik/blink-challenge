<template>
  <div class="search">
    <!--
      USE FONT AWESOME FOR THE SEARCH ICON
    -->
    <link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.7.1/css/all.css" integrity="sha384-fnmOCqbTlWIlj8LyTjo7mOUStjsKC4pOpQbqyi7RrhN7udi9RwhKkMHpvLbHG9Sr" crossorigin="anonymous">
    <!--
      SEARCH BUTTON
    -->
    <div class="search-button" @click="searchDataFromAPI()"><i class="fas fa-search"></i></div>
    <!--
      SEARCH INPUT
    -->
    <div class="search-box">
      <input type="text" placeholder="Search" v-on:keyup.enter="searchDataFromAPI()" v-model="search_text">
    </div>
  </div>
</template>

<script>
//Need this for axios to work in Internet Explorer, also needed to npm install @babel/polyfill
import "@babel/polyfill";

import axios from 'axios'
export default {
  name: 'SearchBar',
  data () {
    return {
      search_text: '',
      search_page_num: 1
    }
  },
  created(){
    /*
      Get the data from the API.
      When the user lands on the page, send inital data to the dashboard.
    */
    this.getDataFromAPI();

    /*
      Check to see when the user scrolled to the bottom of the page.
      If the user does, then gather more data.
    */
    let can_get_data = true;
    window.onscroll = (ev) => {
      if(document.documentElement.scrollHeight - window.innerHeight === document.documentElement.scrollTop && can_get_data){
        callingAPI();
        this.getDataFromAPI();
      }
    };

    /*
      Temporary solution for Internet Explorer.
      In IE, the onscroll event above triggers multiple times when scrolled to the bottom of the page.
      This does not happen in Chrome or Firefox.
    */
    let callingAPI = () => {
      can_get_data = false;
      setTimeout(()=> {
        can_get_data = true;
      },500)
    }
  },
  methods: {
    getDataFromAPI(){
      /*
        Get more data if the search_page_num is greater than zero.
        Since the API returns data in pages, once the last page from the API is loaded, then don't load data again.
      */
      if(this.search_page_num > 0){
        /*
          Get data from the API based on the current page number.
        */
        axios.get("https://swapi.co/api/people/?page=" + this.search_page_num)
        .then(response => {
          /*
            Send the data to the dashboard.
          */
          this.$parent.updateApiData(response.data.results);

          if(response.data.next != null){
            /*
              If the next page is not null, then increase the page count as there can be more data to load.
            */
            this.search_page_num++;
          } else {
            /*
              If the next page is null, then set the page count to zero so we don't try to get data from a non existing page number.
            */
            this.search_page_num = 0;
          }
        })
        .catch(e => {
          console.log('error:', e);
        })
      }
    },
    searchDataFromAPI(){
      /*
        Tell the dashboard to blank the results.
        This is because the user is searching for a specific character and we only want to display the search result.
      */
      this.$parent.blankResults();

      if(this.search_text != ""){
        /*
          If the search from the user is not blank, then use the search feature of the API to get results.

          TODO:
          Check the input to make sure it only contains letters
        */
        axios.get("https://swapi.co/api/people/?search=" + this.search_text)
        .then(response => {
          /*
            Send the results to the dashboard.
          */
          this.$parent.updateApiData(response.data.results);
        })
        .catch(e => {
          console.log('error:', e);
        })
      } else {
        /*
          The search bar is blank.
          Reset the search page count to start at the beginning and run getDataFromAPI().
        */
        this.search_page_num = 1;
        this.getDataFromAPI();
      }
    }
  }
}
</script>

<style scoped>
  .search-box {
    overflow: hidden;
  }
  .search-box input {
    padding: 10px;
    width: 100%;
  }
  .search-button {
    padding: 10px 16px;
    background-color: #303030;
    color: white;
    float: right;
    font-size: 16px;
  }
  .search-button:hover {
    cursor: pointer;
  }
</style>
