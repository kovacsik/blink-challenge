<template>
  <div class="body-item">
    <div class="item-description">
      {{ name }}
      <br>
      <div class="item-info" @click="displayModal()">click here for more info</div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'BodyItem',
  props: ["api_item"],
  data () {
    return {
      name: ""
    }
  },
  created(){
    this.name = this.api_item.name || ""
  },
  methods:{
    displayModal(){
      /*
        Tell the dashboard to display the modal with this character's data
      */
      this.$parent.displaySingleItemModal(this.api_item);
    }
  }
}
</script>

<style scoped>
  .body-item {
    width: 200px;
    height: 200px;
    margin: 4px;
    padding: 20px;
    border: 1px solid lightgray;
    background-image: url("../assets/img/star_wars_logo_2.png");
    background-size: contain;
    display: flex;
  }
  .item-description {
    text-align: center;
    margin: auto;
    font-size: 22px;
  }
  .item-info {
    padding: 10px;
    font-size: 12px;
    font-style: italic;
  }
  .item-info:hover {
    cursor: pointer;
  }
</style>
