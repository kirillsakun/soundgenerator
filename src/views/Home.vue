<template>
  <div class="home">
    <h1>Background sound generator</h1>
    <div class="sounds">
      <SoundElement v-for="el in sounds" :key="el.title" :soundEl="el" :mainVolume="mainVolume" />
    </div>
    <div class="controls">
      <div class="group">
        <button @click="timersShow = !timersShow; mixesShow = false">Timers</button>
        <button @click="mixesShow = !mixesShow; timersShow = false">Mixes</button>
      </div>
      <div class="group">
        <input
          type="range"
          class="main-volume"
          min="0"
          max="1"
          step=".01"
          name="volume"
          id="volume"
          v-model="mainVolume"
          @input="allSoundsChanger()"
        />
        <button
          class="mute-button"
          @click="muteAll(isMuted =! isMuted)"
        >{{isMuted ? 'unmute' : 'mute'}}</button>
      </div>
    </div>
    <transition name="fade">
      <keep-alive>
        <Mixes class="lower-el" v-if="mixesShow" :currentSounds="sounds" :mainVolume="mainVolume" ref="mixes">
          <span class="close" @click="mixesShow = false"></span>
        </Mixes>
      </keep-alive>
    </transition>
    <transition name="fade">
      <keep-alive>
        <Timers class="lower-el" v-if="timersShow" :mainVolume="mainVolume">
          <span class="close" @click="timersShow = false"></span>
        </Timers>
      </keep-alive>
    </transition>
  </div>
</template>

<script>
// @ is an alias to /src
import SoundElement from "@/components/SoundElement.vue";
import Mixes from "@/components/Mixes.vue";
import Timers from "@/components/Timers.vue";
//sounds
import rain1 from "@/assets/audio/rain.mp3";
import rain2 from "@/assets/audio/rain.ogg";
import thunder1 from "@/assets/audio/thunder.mp3";
import thunder2 from "@/assets/audio/thunder.ogg";
import bonfire1 from "@/assets/audio/bonfire.mp3";
import bonfire2 from "@/assets/audio/bonfire.ogg";
import grass1 from "@/assets/audio/grass.mp3";
import grass2 from "@/assets/audio/grass.ogg";
import radio1 from "@/assets/audio/radio.mp3";
import radio2 from "@/assets/audio/radio.ogg";
import sea1 from "@/assets/audio/sea.mp3";
import sea2 from "@/assets/audio/sea.ogg";
import wind1 from "@/assets/audio/wind.mp3";
import wind2 from "@/assets/audio/wind.ogg";

//images
import rainImg from "@/assets/rain.png";
import thunderImg from "@/assets/thunder.png";
import bonfireImg from "@/assets/bonfire.png";
import grassImg from "@/assets/grass.png";
import radioImg from "@/assets/radio.png";
import seaImg from "@/assets/sea.png";
import windImg from "@/assets/wind.png";

export default {
  name: "home",
  components: {
    SoundElement,
    Mixes,
    Timers
  },
  data() {
    return {
      mainVolume: 1,
      prevMainVolume: 1,
      isMuted: false,
      mixesShow: false,
      timersShow: false,
      sounds: [
        {
          title: "rain",
          img: rainImg,
          volume: 0.7,
          isPlaying: false,
          source: [
            {
              src: rain1,
              type: "audio/mpeg"
            },
            {
              src: rain2,
              type: "audio/ogg"
            }
          ]
        },
        {
          title: "thunder",
          img: thunderImg,
          isPlaying: false,
          volume: 0.7,
          source: [
            {
              src: thunder1,
              type: "audio/mpeg"
            },
            {
              src: thunder2,
              type: "audio/ogg"
            }
          ]
        },
        {
          title: "bonfire",
          img: bonfireImg,
          isPlaying: false,
          volume: 0.7,
          source: [
            {
              src: bonfire1,
              type: "audio/mpeg"
            },
            {
              src: bonfire2,
              type: "audio/ogg"
            }
          ]
        },
        {
          title: "grass",
          img: grassImg,
          isPlaying: false,
          volume: 0.7,
          source: [
            {
              src: grass1,
              type: "audio/mpeg"
            },
            {
              src: grass2,
              type: "audio/ogg"
            }
          ]
        },
        {
          title: "radio",
          img: radioImg,
          isPlaying: false,
          volume: 0.7,
          source: [
            {
              src: radio1,
              type: "audio/mpeg"
            },
            {
              src: radio2,
              type: "audio/ogg"
            }
          ]
        },
        {
          title: "sea",
          img: seaImg,
          isPlaying: false,
          volume: 0.7,
          source: [
            {
              src: sea1,
              type: "audio/mpeg"
            },
            {
              src: sea2,
              type: "audio/ogg"
            }
          ]
        },
        {
          title: "wind",
          img: windImg,
          isPlaying: false,
          volume: 0.7,
          source: [
            {
              src: wind1,
              type: "audio/mpeg"
            },
            {
              src: wind2,
              type: "audio/ogg"
            }
          ]
        }
      ]
    };
  },
  methods: {
    allSoundsChanger() {
      this.sounds.forEach(element => {
        document.getElementById(element.title).volume =
          this.mainVolume * element.volume;
      });
      this.mainVolume == 0 ? (this.isMuted = true) : (this.isMuted = false);
    },
    muteAll(isMuted) {
      if (isMuted) {
        this.prevMainVolume = this.mainVolume;
        this.mainVolume = 0;
        this.sounds.forEach(element => {
          document.getElementById(element.title).volume =
            this.mainVolume * element.volume;
        });
      } else {
        this.mainVolume = this.prevMainVolume;
        this.sounds.forEach(element => {
          document.getElementById(element.title).volume =
            this.mainVolume * element.volume;
        });
      }
    }
  }
};
</script>

<style lang="scss" scoped>
.home {
  width: 100%;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  padding-bottom: 3rem;
}
.sounds {
  max-width: 90%;
  display: flex;
  justify-content: space-around;
  flex-wrap: wrap;
}
.controls {
  position: fixed;
  bottom: 0;
  left: 0;
  width: 100%;
  height: 3rem;
  display: flex;
  justify-content: center;
  align-items: center;
  background: rgba(0, 0, 0, 0.65);
  * {
    margin: 0 0.5rem;
  }
}
.group {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 14rem;
}
.main-volume {
  display: block;
  -webkit-appearance: none;
  background-color: #fff;
  width: 6rem;
  height: 0.25rem;
  border-radius: 0.125rem;
  &::-webkit-slider-thumb {
    -webkit-appearance: none;
    background-color: #ffffff;
    width: 1rem;
    height: 1rem;
    border-radius: 50%;
    cursor: pointer;
    box-shadow: rgb(0, 0, 0) 0 0 0.25rem;
  }
}
.lower-el{
  position: fixed;
  bottom: 3rem;
  left: 0;
  width: 100%;
  min-height: 4rem;
  background: rgba(0, 0, 0, 0.65);
  display: flex;
  flex-direction: column;
  align-items: center;
  > .close {
    width: 1.2rem;
    height: 1.2rem;
    display: block;
    position: absolute;
    top: 1rem;
    right: 1rem;
  }
}

@media (max-width: 480px) {
  h1 {
    font-size: 1.8rem;
  }
  .home {
    padding-bottom: 6rem;
  }
  .controls {
    flex-direction: column;
    height: 6rem;
  }
  .group {
    width: 100%;
    height: 3rem;
    * {
      width: 50% !important;
    }
  }
  .lower-el{
    bottom: 6rem;
  }
}
</style>