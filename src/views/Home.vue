<template>
  <div class="home">
    <search-bar :characters="characters" @new-query="getData" @filter-data="getQueryData"/>
    <CharacterList :characters="characters" :filteredData="filteredData"/>
    <div class="sites-wrapper">
      <button v-if="characters.info.prev" @click="loadPrev"><i class="far fa-arrow-alt-circle-left"></i></button>
        <div class="site-number">
          {{ currentSite }} of {{ characters.info.pages }}
        </div>
      <button v-if="characters.info.next" @click="loadNext"><i class="far fa-arrow-alt-circle-right"></i></button>
    </div>
    <a id="scroll-top" :class="{'hidden': hidden}" href="#"><i class="fas fa-angle-double-left"></i></a>
  </div>
</template>

<script>
import CharacterList from '@/components/CharacterList/CharacterList.vue'
import SearchBar from '@/components/SearchBar/SearchBar.vue'

export default {
  name: 'Home',
  data () {
    return {
      characters: {},
      currentSite: 1,
      hidden: true,
      filters: {
        status: '',
        species: '',
        gender: []
      }
    }
  },
  components: {
    CharacterList,
    SearchBar
  },
  methods: {
    async getData (data) {
      console.log('fired')
      this.characters = data
      this.currentSite = 1
    },
    async getQueryData (data) {
      this.filters.status = data.status
      this.filters.species = data.species
      this.filters.gender = data.gender
    },
    async loadNext () {
      const req = await fetch(`${this.characters.info.next}`)
      const data = await req.json()
      this.characters = data
      this.currentSite += 1
      window.scrollTo(0, 0)
    },
    async loadPrev () {
      const req = await fetch(`${this.characters.info.prev}`)
      const data = await req.json()
      this.characters = data
      this.currentSite -= 1
      window.scrollTo(0, 0)
    },
    getTop () {
      if (document.documentElement.scrollTop > 100) {
        this.hidden = false
        return
      }
      this.hidden = true
    }

  },
  computed: {
    filteredData () { // here are all the queries that can be inputed by user
      const res = this.characters.results
      return res.filter((character) => {
        const status = character.status
        const species = character.species
        const gender = character.gender

        const statusQ = this.filters.status
        const speciesQ = this.filters.species
        const genderQ = this.filters.gender
        const result = () => { // searching for characters that have at least one element from gender query
          if (genderQ.length < 1) {
            return true
          }
          return genderQ.some((item) => item === gender)
        }

        return status.includes(statusQ) && species.includes(speciesQ) && result()
      })
    }
  },
  async created () {
    const req = await fetch('https://rickandmortyapi.com/api/character')
    const data = await req.json()
    this.characters = data
    this.currentSite = 1

    window.addEventListener('scroll', this.getTop)
  },
  beforeUnmount () {
    window.removeEventListener('scroll', this.getTop)
  }

}
</script>
<style>
.home {
  min-width: 100%;
}
.sites-wrapper {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-top: 3.5rem;
  padding-bottom: 3.5rem;
  font-size: 1.5rem;
  color: rgb(156, 192, 156);
}
.sites-wrapper button {
  font-size: 2.5rem;
  border: none;
  background: none;
  cursor: pointer;
  color: #d8ced3;
  margin: 0 0.5rem;

}
#scroll-top {

  width: 60px;
    height: 60px;
    background-color: #36352a;
    position: fixed;
    bottom: 1rem;
    right: 1rem;
    z-index: 100;
    color: white;
    font-size: 2.5rem;
    border-radius: 10px;
    box-shadow: 0 0 3px hsl(180, 4%, 36%);

    transition: opacity 0.5s ease-out, box-shadow 0.2s ease-out;
}
#scroll-top:hover {
  box-shadow: 0 0 15px hsl(177, 37%, 65%);
}
#scroll-top i {
  transform: rotateZ(90deg);
  line-height: 60px;

  }
</style>
