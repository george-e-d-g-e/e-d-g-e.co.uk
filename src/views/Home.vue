<template>
  <div class="home">
    <!-- <HelloWorld msg="Welcome to Your Vue.js App"/> -->
    <!-- <Projects heading="Projects"/> -->
    <Portfolio/>
  </div>
</template>

<script>
// @ is an alias to /src
// import HelloWorld from "@/components/HelloWorld.vue";
//import Projects from "@/components/Projects.vue";
import Portfolio from "@/components/Portfolio.vue";

export default {
  name: "home",
  components: {
    // HelloWorld
   // Projects
    Portfolio
  }
};
</script>
