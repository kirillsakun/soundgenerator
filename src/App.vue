<template>
  <div id="app">
    <nav id="main-menu">
      <h2 @click="aboutShow = false">Home</h2>
      <h2 @click="aboutShow = true">About me</h2>
    </nav>
    <Home />
    <transition name="fade">
      <About v-if="aboutShow" />
    </transition>
  </div>
</template>
<script>
import Home from "./views/Home.vue";
import About from "./views/About.vue";
export default {
  components: {
    Home,
    About
  },
  data() {
    return {
      aboutShow: false
    };
  }
};
</script>

<style lang="scss">
@import url("https://fonts.googleapis.com/css?family=Viga&display=swap");
@import url("https://fonts.googleapis.com/css?family=Ubuntu:400,500&display=swap");

*,
*::before,
*::after {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  cursor: inherit;
  position: relative;
  font-family: "Ubuntu", sans-serif;
  color: white;
}

h1 {
  margin-top: 4rem;
  font-family: "Viga", sans-serif;
  font-weight: 400;
  font-size: 3rem;
  text-align: center;
  cursor: default;
}
#app {
  color: white;
  background-image: url(./assets/mar.jpg),
  linear-gradient(to right, rgba(0, 0, 0, 0.3), rgba(0, 0, 0, 0.3));
  background-blend-mode: multiply;
  background-repeat: no-repeat;
  background-position: center;
  background-size: cover;
  background-attachment: fixed;
  min-height: 100vh;
}
#main-menu {
  position: absolute; 
  display: flex;
  top: 0rem;
  right: 0rem;
  z-index: 100;
  h2 {
    font-weight: 400;
    font-size: 1.2rem;
    margin: 0.5rem 1rem;
    padding: 0.5rem;
    cursor: pointer;
  }
}
input,
button{
  cursor: pointer;
}

button {
  background: none;
  border-radius: 0.25rem;
  border: 2px white solid;
  padding: 0.5rem;
  width: 6.25rem;
  text-transform: uppercase;
}
.close{
  background-image: url('data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHhtbG5zOnhsaW5rPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5L3hsaW5rIiB2ZXJzaW9uPSIxLjEiIGlkPSJDYXBhXzEiIHg9IjBweCIgeT0iMHB4IiB2aWV3Qm94PSIwIDAgNDcuOTcxIDQ3Ljk3MSIgc3R5bGU9ImVuYWJsZS1iYWNrZ3JvdW5kOm5ldyAwIDAgNDcuOTcxIDQ3Ljk3MTsiIHhtbDpzcGFjZT0icHJlc2VydmUiIHdpZHRoPSI1MTIiIGhlaWdodD0iNTEyIiBjbGFzcz0iIj48ZyB0cmFuc2Zvcm09Im1hdHJpeCgxIDAgMCAxIDAgMCkiPjxnPgoJPHBhdGggZD0iTTI4LjIyOCwyMy45ODZMNDcuMDkyLDUuMTIyYzEuMTcyLTEuMTcxLDEuMTcyLTMuMDcxLDAtNC4yNDJjLTEuMTcyLTEuMTcyLTMuMDctMS4xNzItNC4yNDIsMEwyMy45ODYsMTkuNzQ0TDUuMTIxLDAuODggICBjLTEuMTcyLTEuMTcyLTMuMDctMS4xNzItNC4yNDIsMGMtMS4xNzIsMS4xNzEtMS4xNzIsMy4wNzEsMCw0LjI0MmwxOC44NjUsMTguODY0TDAuODc5LDQyLjg1Yy0xLjE3MiwxLjE3MS0xLjE3MiwzLjA3MSwwLDQuMjQyICAgQzEuNDY1LDQ3LjY3NywyLjIzMyw0Ny45NywzLDQ3Ljk3czEuNTM1LTAuMjkzLDIuMTIxLTAuODc5bDE4Ljg2NS0xOC44NjRMNDIuODUsNDcuMDkxYzAuNTg2LDAuNTg2LDEuMzU0LDAuODc5LDIuMTIxLDAuODc5ICAgczEuNTM1LTAuMjkzLDIuMTIxLTAuODc5YzEuMTcyLTEuMTcxLDEuMTcyLTMuMDcxLDAtNC4yNDJMMjguMjI4LDIzLjk4NnoiIGRhdGEtb3JpZ2luYWw9IiMwMDAwMDAiIGNsYXNzPSJhY3RpdmUtcGF0aCIgc3R5bGU9ImZpbGw6I0ZGRkZGRiIgZGF0YS1vbGRfY29sb3I9IiMwMDAwMDAiPjwvcGF0aD4KPC9nPjwvZz4gPC9zdmc+');
  background-size: 100%;
}
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.7s ease;
}

.fade-enter,
.fade-leave-to {
  opacity: 0;
}
.fade-enter-to,
.fade-leave {
  opacity: 1;
}
</style>
