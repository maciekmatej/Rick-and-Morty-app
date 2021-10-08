<template>
  <div  class="card-wrapper" @click="showInfo">
    <div class="card-inner" :class="{'flip': tab == character.id}">
        <div class="card-front">
            <img :src="character.image" alt="character_image">
            <h2>[{{ character.name }}]</h2>

        </div>
        <div class="card-back" >
          <div class="bg"
              :class="{'rotate': tab == character.id}">
          </div>
          <div class="card-info">
            <span>GENDER: {{ character.gender }}</span>
            <span>STATUS: {{ character.status }}</span>
            <span>SPECIES: {{ character.species }}</span>
            <span>TYPE: {{ character.type }}</span>
            <span>CREATED: {{ formatedDate }}</span>

            </div>
        </div>
    </div>
  </div>
</template>

<script>
// @click="showInfo"  <= this was used to flip clicked card but I chose a function with classes in the end.
export default {
  data () {
    return {

    }
  },
  name: 'Card',
  props: ['character', 'tab'],
  emits: ['show-tab'],
  methods: {
    showInfo () {
      if (this.tab === this.character.id) {
        this.$emit('show-tab', 0)
      } else {
        this.$emit('show-tab', this.character.id)
      }
    }
  },
  computed: {
    formatedDate () {
      const dateParts = this.character.created.split('-')
      const dayPart = dateParts[2].split('T')
      const day = dayPart[0]
      const month = dateParts[1]
      const year = dateParts[0]
      return `${day}-${month}-${year}`
    }
  }

}

</script>

<style>

.card-inner {
    position: relative;
    transition: transform 0.5s;
    width: 100%;
    height: 100%;
    transform-style: preserve-3d;
    box-shadow: 0 0 15px #5cad4a;
    border-radius: 20px;
}
.card-wrapper {
    width: 100%;
    height: 100%;
    perspective: 800px;
    cursor: pointer;

}
.card-front {
  position: relative;
}
.card-front, .card-back {

    width: 100%;
    height: 100%;
    backface-visibility: hidden;
    display: flex;
    flex-direction: column;
    border-radius: 20px;
    background-color: #5cad4a;
    color: white;

}
.card-back {
    position: absolute;
    top: 0;
    transform: rotateY(180deg);
    background-color: rgba(0, 128, 128, 0.274);
    justify-content: space-around;
    overflow: hidden;

}
.bg {
    position: absolute;
    top: -10%;
    left: -35%;
    min-width: 26rem;
    min-height: 26rem;
    background-image: url(~@/assets/images/portal1.png);
    background-repeat: no-repeat;
    background-size: cover;
    background-position: 50% 50%;

}
.card-front h2 {
  min-height: 3.5rem;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 1.2rem;
  font-weight: 700;
}
.card-info {
  z-index: 5;
  display: flex;
  flex-direction: column;
  height: 100%;
  align-items: center;
  justify-content: center;
  font-size: 1rem;
  font-weight: 400;

}
.card-info span {
  background:none;
  padding: 5px;
  border-radius: 3px;
}

.flip {
  transform: rotateY(180deg);
}
.rotate {
   animation: 30s spin infinite linear;
}

.card-front img {
  border-top-left-radius: 20px;
  border-top-right-radius: 20px;
  -webkit-user-drag: none;
}
.card span {
    display: block;
}
@keyframes spin {
  from {
    transform: rotateZ(0deg);
  }

  to {
    transform: rotateZ(360deg);
  }
}

</style>
