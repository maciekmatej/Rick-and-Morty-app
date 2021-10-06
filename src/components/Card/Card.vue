<template>
<div @click="showInfo" class="card-wrapper">
    <div class="card-inner" :class="{'flip': activeTab}">
        <div class="card-front">
            <img :src="character.image" alt="character_image">
            <h2>[ {{ character.name }} ]</h2>

        </div>
        <div class="card-back" >
          <div class="bg" :class="{'rotate': activeTab}">
          </div>
          <div class="card-info">
            <span>Gender: {{ character.gender }}</span>
            <span>Status: {{ character.status }}</span>
            <span>Species: {{ character.species }}</span>
            <span>Type: {{ character.type }}</span>
            <span>Created: {{ character.created }}</span>

            </div>
        </div>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      activeTab: false
    }
  },
  name: 'Card',
  props: ['character'],
  methods: {
    showInfo () {
      this.activeTab = !this.activeTab
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
    box-shadow: 0 0 10px rgb(145, 145, 145);
    border-radius: 20px;
}
.card-wrapper {
    width: 100%;
    height: 100%;
    perspective: 800px;

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
    background-color: white;

}
.card-back {
    position: absolute;
    top: 0;
    transform: rotateY(180deg);
    background-color: rgba(0, 128, 128, 0.274);
    width: 100%;
    height: 100%;
    justify-content: space-around;
    overflow: hidden;

}
.bg {
    position: absolute;
    top: -20%;
    left: -50%;
    min-width: 30rem;
    min-height: 30rem;
    background-image: url(~@/assets/images/portal.png);
    background-repeat: no-repeat;
    background-size: cover;
    background-position: 50% 50%;

}
.card-info {
  z-index: 5;
  display: flex;
  flex-direction: column;
  height: 100%;
  align-items: center;
  justify-content: space-around;
  font-size: 1rem;
  font-weight: 700;
  color: black;
}
.card-info span {
  background-color: white;
  padding: 5px;
  border-radius: 5px;
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
