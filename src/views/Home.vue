<template>
  <div class="home">
    <search-bar @new-query="getData"/>
    <CharacterList :characters="characters"/>
    <div class="sites-wrapper">
    <button v-if="characters.info.prev" @click="loadPrev"><i class="far fa-arrow-alt-circle-left"></i></button>
      <div class="site-number">{{ currentSite }} of {{ characters.info.pages }}</div>
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
      hidden: true
    }
  },
  components: {
    CharacterList,
    SearchBar
  },
  methods: {
    async getData (data) {
      this.characters = data
      this.currentSite = 1
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
  margin-top: 2rem;
  padding-bottom: 1rem;
  font-size: 1.5rem;
}
.sites-wrapper button {
  font-size: 2.5rem;
  border: none;
  background: none;
  cursor: pointer;
  color: #46383f;
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
    font-size: 3rem;
    border-radius: 10px;
    box-shadow: 0 0 2px hsl(72, 5%, 20%);

    transition: opacity 0.5s ease-out, box-shadow 0.2s ease-out;
}
#scroll-top:hover {
  box-shadow: 0 0 10px hsl(75, 2%, 36%);
}
#scroll-top i {
  transform: rotateZ(90deg);
  line-height: 60px;

  }
</style>
