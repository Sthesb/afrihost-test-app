<template>

  <div class="app">
    <Navigation />
    <!-- <nav>
      <router-link to="/">Home</router-link> |
      <router-link to="/about">About</router-link>
    </nav> -->
    
    <div class="container">
      <div class="sidebar"></div>
      <div class="content">
        <router-view/>
      </div>
    </div>
  </div>
</template>

<script>
import Navigation from './components/Navigation'

export default {
  components: { Navigation },
}
</script>



<style>
@import url('https://fonts.googleapis.com/css2?family=Rubik:ital,wght@0,300;0,400;0,500;0,700;0,900;1,300;1,500;1,700;1,900&display=swap');
* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
  font-family: 'Rubik', sans-serif;
  
}

.app {
  min-height: 100vh;
  position: relative;
  background-color: #f1f1f1;
}

.container {
  padding: 0 20px;
  max-width: 1140px;
  margin: 0 auto;
}


</style>
