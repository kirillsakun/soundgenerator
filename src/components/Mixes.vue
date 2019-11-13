<template>
  <div id="mixes">
    <h3>Mixes</h3>
    <slot></slot>
    <ul v-if="!!mixes">
      <li v-for="(mix, id) in mixes" :key="mix.title">
        <h5>{{mix.title}}</h5>
        <button @click="toggle(mix.isActive = ! mix.isActive, id)">{{mix.isActive ? 'off' : 'on'}}</button>
        <span @click="deleteMix(id)" class="close"></span>
      </li>
    </ul>
    <p v-else>There are no mixes</p>
    <div class="controls">
      <button @click="clearAll()">Clear all</button>
      <div id="new-mix">
        <input
          type="text"
          name="mix-title"
          id="new-mix-title"
          placeholder="Enter mix title"
          v-model="newMixTitle"
        />
        <button @click="addNewMix();">Add new</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Mixes",
  created() {
    if (localStorage.getItem("mixes") === null) {
      localStorage.setItem(
        "mixes",
        JSON.stringify([
          {
            title: "Rainy day",
            isActive: false,
            sounds: {
              thunder: 0.5,
              wind: 0.3,
              rain: 0.8
            }
          },
          {
            title: "Night of the bonfire",
            isActive: false,
            sounds: {
              grass: 0.05,
              wind: 0.2,
              bonfire: 0.8
            }
          }
        ])
      );
    this.mixes = JSON.parse(localStorage.getItem("mixes"));
    }
  },
  props: {
    currentSounds: Array,
    mainVolume: [Number, String]
  },
  data() {
    return {
      mixes: [],
      newMixTitle: ""
    };
  },
  methods: {
    addNewMix() {
      if (
        this.newMixTitle !== "" &&
        this.mixes.filter(mix => {
          return mix.title == this.newMixTitle;
        }).length === 0
      ) {
        let mixSounds = {};
        this.currentSounds.forEach(sound => {
          if (sound.isPlaying) {
            mixSounds[sound.title] = sound.volume;
          }
        });
        this.mixes.push({
          title: this.newMixTitle,
          isActive: false,
          sounds: mixSounds
        });

        localStorage.setItem("mixes", JSON.stringify(this.mixes));
      }
    },
    toggle(state, id) {
      let currentMix = this.mixes[id];
      this.mixes.forEach(element => {
        if (element.isActive) {
          element.isActive = false;
          this.currentSounds.forEach(el => {
            if (Object.keys(element.sounds).includes(el.title)) {
              el.isPlaying = false;
              document.getElementById(el.title).pause();
            }
          });
        }
      });
      currentMix.isActive = state;
      this.currentSounds.forEach(el => {
        if (Object.keys(currentMix.sounds).includes(el.title)) {
          if (state) {
            el.volume = currentMix.sounds[el.title];
            document.getElementById(el.title).volume =
              currentMix.sounds[el.title] * this.mainVolume;
            el.isPlaying = true;
            document.getElementById(el.title).play();
          } else {
            el.isPlaying = false;
            document.getElementById(el.title).pause();
          }
        } else {
          el.isPlaying = false;
        }
      });
    },
    deleteMix(id) {
      this.mixes.splice(id, 1);
      localStorage.setItem("mixes", JSON.stringify(this.mixes));
    },
    clearAll() {
      this.mixes = [];
      localStorage.setItem("mixes", JSON.stringify(this.mixes));
    },
    refresh() {
      this.mixes.forEach(element => {
        if (element.isActive) {
          element.isActive = false;
        }
      });
    }
  }
};
</script>

<style scoped lang="scss">
#mixes {

  h3 {
    margin: 1rem 0;
  }
  li {
    display: flex;
    width: 100%;
    align-items: center;
    * {
      margin: 0.5rem;
    }
    h5 {
      font-size: 1.2rem;
      min-width: 12rem;
    }
    .close {
      width: 0.8rem;
      height: 0.8rem;
      background-image: url("data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHhtbG5zOnhsaW5rPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5L3hsaW5rIiB2ZXJzaW9uPSIxLjEiIGlkPSJDYXBhXzEiIHg9IjBweCIgeT0iMHB4IiB2aWV3Qm94PSIwIDAgNDcuOTcxIDQ3Ljk3MSIgc3R5bGU9ImVuYWJsZS1iYWNrZ3JvdW5kOm5ldyAwIDAgNDcuOTcxIDQ3Ljk3MTsiIHhtbDpzcGFjZT0icHJlc2VydmUiIHdpZHRoPSI1MTIiIGhlaWdodD0iNTEyIiBjbGFzcz0iIj48ZyB0cmFuc2Zvcm09Im1hdHJpeCgxIDAgMCAxIDAgMCkiPjxnPgoJPHBhdGggZD0iTTI4LjIyOCwyMy45ODZMNDcuMDkyLDUuMTIyYzEuMTcyLTEuMTcxLDEuMTcyLTMuMDcxLDAtNC4yNDJjLTEuMTcyLTEuMTcyLTMuMDctMS4xNzItNC4yNDIsMEwyMy45ODYsMTkuNzQ0TDUuMTIxLDAuODggICBjLTEuMTcyLTEuMTcyLTMuMDctMS4xNzItNC4yNDIsMGMtMS4xNzIsMS4xNzEtMS4xNzIsMy4wNzEsMCw0LjI0MmwxOC44NjUsMTguODY0TDAuODc5LDQyLjg1Yy0xLjE3MiwxLjE3MS0xLjE3MiwzLjA3MSwwLDQuMjQyICAgQzEuNDY1LDQ3LjY3NywyLjIzMyw0Ny45NywzLDQ3Ljk3czEuNTM1LTAuMjkzLDIuMTIxLTAuODc5bDE4Ljg2NS0xOC44NjRMNDIuODUsNDcuMDkxYzAuNTg2LDAuNTg2LDEuMzU0LDAuODc5LDIuMTIxLDAuODc5ICAgczEuNTM1LTAuMjkzLDIuMTIxLTAuODc5YzEuMTcyLTEuMTcxLDEuMTcyLTMuMDcxLDAtNC4yNDJMMjguMjI4LDIzLjk4NnoiIGRhdGEtb3JpZ2luYWw9IiMwMDAwMDAiIGNsYXNzPSJhY3RpdmUtcGF0aCIgc3R5bGU9ImZpbGw6I0ZGRkZGRiIgZGF0YS1vbGRfY29sb3I9IiMwMDAwMDAiPjwvcGF0aD4KPC9nPjwvZz4gPC9zdmc+");
      background-size: 100%;
    }
  }
  .controls {
    display: flex;
    justify-content: center;
    flex-wrap: wrap;
    button {
      margin: 0.5rem;
    }
    #new-mix {
      min-width: 18.25rem;
      input {
        width: 10rem;
        height: 2.2rem;
        background: none;
        border: 2px white solid;
        border-radius: 0.25rem;
        margin: 0.5rem;
        padding-left: 0.5rem;
      }
    }
  }
}
@media (max-width: 480px) {
  #mixes {
    //bottom: 6rem;
    ul li {
      h5 {
        font-size: 1rem;
        min-width: 10rem;
      }
      button {
        width: 5rem;
      }
    }
  }
}
</style>
