<template>
  <div >
    <div class="character-list" >
      <Card v-for="character in filteredData"
            :key="character.id"
            :character="character"
            :tab="tab"
            @show-tab="activeTab"
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
      tab: ''
    }
  },
  components: { Card },
  props: {
    characters: {
      type: Object
    },
    filteredData: {
      type: Array
    }
  },
  methods: {
    activeTab (data) {
      // eslint-disable-next-line no-undef
      this.tab = data
    }
  }
}
</script>

<style>
  .character-list {
  display: grid;
  width: 70vw;
  min-width: 320px;
  max-width: 1440px;
  margin: 0 auto;
  padding-top: 15rem ;
  grid-gap: 30px;
  grid-template-columns: repeat(auto-fill, minmax(250px, 250px));
  grid-template-rows: repeat(auto-fill, minmax(250px, 1fr));
  border-radius: 20px;
  background: none;
  justify-content: center;
}
@media screen and (max-width: 1159px) {
  .character-list {
    padding-top: 4rem;
  }
}
</style>
