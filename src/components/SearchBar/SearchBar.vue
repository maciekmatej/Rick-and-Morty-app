<template>
  <header class="search-bar">
      <nav>
          <form @change="newQuery" id="query">
              <label for="nameQ">name:</label>
              <input type="text" name="name" id="nameQ">
              <label for="statusQ">status:</label>
              <select name="status" id="statusQ">
                  <option value="">all</option>
                  <option value="alive">alive</option>
                  <option value="dead">dead</option>
                  <option value="unknown">unknown</option>
              </select>
              <button @click.prevent>Search</button>
          </form>

      </nav>
  </header>
</template>

<script>
export default {
  data () {
    return {
      newData: {}
    }
  },
  name: 'SearchBar',
  emits: ['new-query'],
  methods: {
    async newQuery () {
      const API = 'https://rickandmortyapi.com/api/character/?'
      const form = document.querySelector('#query')
      const res = await fetch(`${API}name=${form.elements.name.value}&status=${form.elements.status.value}`)
      const data = await res.json()
      console.log(res)
      this.newData = data
      this.$emit('new-query', data)
    }
  }

}
</script>

<style>
.search-bar {
    position: fixed;
    top: 0;
    z-index: 9999;
    background-color: white;
    width: 100%;
    height: 6rem;
    box-shadow: 0 0 10px rgb(197, 197, 197);

}
</style>
