<template>
  <div id="app">
    <div class="header">
      <p>This is the header</p>
    </div>
    <div class="sidebar">
      <nav>
        <ul class="overlay_style">
          <a class="closebtn">&times;</a>
          <li>Item one</li>
          <li>Item two</li>
          <li>Item three</li>
        </ul>
      </nav>
    </div>
    <div class="body">
      <p>Lorem ipsum dolor sit amet consectetur adipisicing elit. Corrupti id quis asperiores animi quo quod adipisci sed laudantium officia, nihil vero unde iusto amet rerum atque libero provident soluta esse?</p>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      image: [
        {
          id: 0,
          name: "picture1",
          sourceUrl: "./pictures/2019-07-15 11.15.42 1.jpg",
          component: "my Art",
          date: "dd-mm-yy"
        }
      ]
    };
  }
};
</script>

<style lang="scss">
body {
  padding: 0;
  margin: 0;
  position: relative;
}
#app {
  .header {
    border: 1px solid gray;
    position: relative;
  }
  .sidebar {
    position: relative;
    nav {
      position: absolute;
      .overlay_style {
        position: fixed;
        bottom: 0;
        top: 35px;
        width: 15%;
        // top: 100px;
        border: 1px solid gray;
      }
    }
  }
  .body {
    width: 80%;
    margin-left: 350px;
    // height: 100vh;
    background-color: red;
  }
}
</style>