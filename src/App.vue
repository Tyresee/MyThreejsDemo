<!--
 * @Author: ttc
-->
<template>
  <div id="app">
    <!--    <DrawingBoard/>-->
    <!--    <ChatGen></ChatGen>-->
    <ChatGenWithXYZ></ChatGenWithXYZ>
    <!--    <ChatGenWithLight></ChatGenWithLight>-->
  </div>
</template>

<script>
// import DrawingBoard from './components/DrawingBoard.vue'
// import ChatGen from "@/components/ChatGen";
// import ChatGenWithXYZ from "@/components/ChatGenWithXYZ";
import ChatGenWithXYZ from "@/components/ChatGenWithXYZ";

export default {
  name: 'App',
  components: {
    // DrawingBoard,
    // ChatGen,
    // ChatGenWithXYZ
    ChatGenWithXYZ
  },
  mounted() {
  }
}
</script>

<style>
#app {
  width: 100%;
  height: 100%;

}

body {
  margin: 0;
}
</style>
