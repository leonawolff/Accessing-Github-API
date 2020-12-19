
<template>
  <div id="app">
    <img alt="frog" src="./assets/Frog.gif">
    <h1>GitHub API Visualiser</h1>
    <form>
      <label for="fname">Github username:</label><br/><br/>
      <input type="text" id="username" name="username"><br/><br/>
      <input type="submit" value="Go!">
    </form>
  </div>
</template>





<script>
export default {
  name: 'App'
}
</script>


<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
