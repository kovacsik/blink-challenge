<template>
  <div>
    <!--
      MODAL BACKGROUND
     -->
    <div class="modal-background" @click="closeModal()"></div>

    <!--
      MODAL
     -->
    <div class="modal">
      <div class="character-name">
        {{ character_item.name }}
      </div>
      <div class="more-info">
        <a :href="character_item.url" target="_blank">more info on swapi.co</a>
      </div>
      <div class="character-details">
        <div>Quick Info:</div>
        <ul>
          <li><strong>Height: </strong>{{ character_item.height || "n/a" }}</li>
          <li><strong>Mass: </strong>{{ character_item.mass || "n/a" }}</li>
          <li><strong>Hair Color: </strong>{{ character_item.hair_color || "n/a" }}</li>
          <li><strong>Skin Color: </strong>{{ character_item.skin_color || "n/a" }}</li>
          <li><strong>Eye Color: </strong>{{ character_item.eye_color || "n/a" }}</li>
          <li><strong>Birth Year: </strong>{{ character_item.birth_year || "n/a" }}</li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ItemModal',
  props: ["character_item"],
  methods: {
    closeModal(){
      /*
        Tell the dashboard to close the modal
      */
      this.$parent.display_item_modal = false;
    }
  }
}
</script>

<style scoped>
  .modal-background {
    position: fixed;
    top: 0;
    left: 0;
    width: 100vw;
    height: 100vh;
    background-color: rgba(0,0,0,0.25);
    z-index: 90;
  }
  .modal {
    position: fixed;
    min-width: 200px;
    min-height: 200px;
    overflow-y: auto;
    width: 50vw;
    height: 50vh;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    margin: auto;
    border: 1px solid lightgray;
    border-radius: 5px;
    /* background-color: white; */
    background-image: url("../assets/img/star_wars_logo_2.png");
    background-repeat: repeat;
    background-size: 15%, 15%;
    z-index: 100;
  }
  .more-info {
    text-align: center;
    font-style: italic;
  }
  .more-info:hover {
    cursor: pointer;
  }
  .character-name {
    text-align: center;
    font-size: 3em;
    border-bottom: 1px solid lightgray;
    padding: 10px 0;
  }
  .character-details {
    padding: 20px;
    font-size: 1.25em;
  }
  .character-details ul {
    list-style-type: none;
    margin: 0;
    padding: 0;
  }
  .character-details ul li {
    padding: 6px;
  }
</style>
