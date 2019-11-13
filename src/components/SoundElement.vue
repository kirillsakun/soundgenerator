<template>
  <div class="audio"
  :style="{backgroundImage: 'url('+ soundEl.img +')'}"
  @click.self="onPlay(soundEl.isPlaying =! soundEl.isPlaying)"
  :class="{active : soundEl.isPlaying }">
    <audio loop preload="none"
    :id="soundEl.title">
      <source v-for="item in soundEl.source"
      :key="item.type"
      :src="item.src" 
      :type="item.type">
    </audio>
    <input type="range"
    class="volume"
    v-model='soundEl.volume'
    @input="volumeChanger()"
    min="0" max="1" step=".01"
    :name="soundEl.title+'-volume'"
    :id="soundEl.title+'-volume'">
  </div>
</template>

<script>
export default {
  name: "SoundElement",
  props: {
    soundEl: Object,
    mainVolume: [String, Number]
  },
  methods:{
    onPlay(isPlaying){
      isPlaying ? document.getElementById(this.soundEl.title).play()
      : document.getElementById(this.soundEl.title).pause();
    },
    volumeChanger(){
      document.getElementById(this.soundEl.title).volume = this.mainVolume * this.soundEl.volume;
      this.$parent.$refs.mixes.refresh();
    }
  },
};
</script>

<style scoped lang="scss">
.audio{
  width: 12rem;
  height: 12rem;
  margin: 1rem;
  border-radius: 1rem;
  transition: .5s;
  cursor: pointer;
  background-size: 9rem;
  background-repeat: no-repeat;
  background-position: center;
}
.audio.active{
  box-shadow: 0px 0px 1rem rgba(255, 255, 255, 0.712);
}
.volume{
  position: absolute;
  bottom: .5rem;
  left: 5%;
  display: block;
  -webkit-appearance: none;
  background-color: #fff;
  width: 90%;
  height: .25rem;
  border-radius: .125rem;
  &::-webkit-slider-thumb {
    -webkit-appearance: none;
    background-color: #ffffff;
    width: 1rem;
    height: 1rem;
    border-radius: 50%;
    cursor: pointer;
    box-shadow: rgb(0, 0, 0) 0 0 .25rem;
  }
}
@media (max-width: 800px) {
  .audio{
    width: 10rem;
    height: 10rem;
    background-size: 7rem;
    margin-bottom: 1.5rem;
  }
  .volume{
    position: absolute;
  }
}
@media (max-width: 500px) {
  .audio{
    width: 8rem;
    height: 8rem;
    background-size: 5.5rem;
    margin-bottom: 1.5rem;
  }
}
</style>
