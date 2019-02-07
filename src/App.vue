<template>
  <div id="app">
    <!--
      BACKGROUND IMAGE
     -->
    <div class="background-image"></div>

    <!--
      DASHBOARD
     -->
    <div class="dashboard-body">
      <!--
        DASHBOARD TITLE
       -->
      <div class="title">
        <Title />
      </div>
      <!--
        SEARCH BAR
       -->
      <div class="search-bar">
        <SearchBar />
      </div>
      <!--
        CHARACTER ITEMS
       -->
      <div class="items">
        <BodyItem v-for="item in api_data" :key="item.name" :api_item="item" />
      </div>
      <!--
        MODAL TO DISPLAY INDIVIDUAL CHARACTER DATA
       -->
      <div v-if="display_item_modal">
        <ItemModal :character_item="item_modal_data" />
      </div>
    </div>
  </div>
</template>

<script>
import Title from './components/Title'
import SearchBar from './components/SearchBar'
import BodyItem from './components/BodyItem'
import ItemModal from './components/ItemModal'

export default {
  name: 'App',
  data () {
    return {
      display_item_modal: false,
      item_modal_data: [],
      api_data: []
    }
  },
  methods: {
    updateApiData(new_data){
      /*
        Whenever there is new data coming from the SearchBar component, add it to the api_data variable.
        This is mainly for the dynamic loading of data when the user scrolls to the bottom of the page
      */
      for(let i = 0; i < new_data.length; i++){
        this.api_data.push(new_data[i]);
      }
    },
    blankResults(){
      /*
        Blank the data.
        This is to make sure the dashboard is displaying correct data when using the search box.
        The search box will call this method if the dashboard needs to refresh the list
      */
      this.api_data = [];
    },
    displaySingleItemModal(item_data){
      /*
        Display a modal.
        Whenever the user clicks on an item in the BodyItem component, display the data from the component.
      */
      this.item_modal_data = item_data;
      this.display_item_modal = true;
    }
  },
  components: {
    Title, SearchBar, BodyItem, ItemModal
  }
}
</script>

<style>
html, body {
  margin: 0;
  padding: 0;
}
#app {
  font-family: 'Calibri', Helvetica, Arial, sans-serif;
}
.background-image {
  background-image: url("./assets/img/star_wars_bg.jpg");
  height: 400px;
  background-attachment: fixed;
  background-position: center;
  background-repeat: no-repeat;
  background-size: cover;
}

.dashboard-body {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  width: 75%;
  margin: auto;
}

.title {
  color: white;
  height: 380px;
  overflow: hidden;
}

.search-bar {
  width: 100%;
}

.items {
  width: 100%;
  margin: 40px 0;
  display: flex;
  flex-wrap: wrap;
  justify-content: flex-start;
}
</style>
