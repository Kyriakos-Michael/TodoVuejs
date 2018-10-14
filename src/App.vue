<template>
    <div id="app">
        <div class="container">
            <div class="row">
                <div class="col">
                    <h1>TODOs</h1>
                </div>
            </div>
            <Todo />

        </div>
    </div>
</template>

<script>
import Todo from "./components/Todo.vue";

export default {
  name: "app",
  components: {
    Todo
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
