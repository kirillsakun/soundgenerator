<template>
  <div id="about">
    <h2> About me</h2>
    <p>Hi, I'm Kirill Sakun, creator of this site.
      <br>
      Enjoy!
    </p>
  </div>
</template>
<style lang="scss" scoped>
  #about{
    position: fixed;
    top: 0;
    left: 0%;
    height: calc(100vh - 3rem);
    width: 100vw;
    background: rgba(0, 0, 0, 0.9);
    z-index: 30;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    h2{
      margin: 2rem;
    }
    p{
      font-size: 1.3rem;
      text-align: center;
    }
  }
</style>