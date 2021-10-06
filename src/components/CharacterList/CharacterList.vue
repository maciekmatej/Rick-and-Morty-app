<template>
  <div>
    <div class="character-list">
      <Card v-for="character in characters.results"
            :key="character.id"
            :character="character"
            />
    </div>
  </div>
</template>
<script>
import Card from '@/components/Card/Card.vue'

export default {
  name: 'CharacterList',
  data () {
    return {

    }
  },
  components: { Card },
  props: {
    characters: {
      type: Object
    }
  },
  methods: {

  }
}
</script>

<style>
  .character-list {
  display: grid;
  width: 70vw;
  min-width: 320px;
  margin: 0 auto;
  padding-top: 8rem ;
  grid-gap: 20px;
  grid-template-columns: repeat(auto-fill, minmax(250px, 250px));
  grid-template-rows: repeat(auto-fill, minmax(250px, 1fr));
  border-radius: 20px;
  cursor: pointer;
  background-color: rgb(243, 243, 243);
  justify-content: center;
}
</style>
