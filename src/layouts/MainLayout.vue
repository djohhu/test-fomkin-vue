<template>
  <div class="app-content">
    <Header/>
    <main>
      <Home/>
    </main>
    <Footer/>
  </div>
</template>

<script>
    import Header from "../components/Header";
    import Home from "../views/Home";
    import Footer from "../components/Footer";

    export default {
        name: "MainLayout",
        components: {Footer, Header, Home}
    }
</script>

<style scoped>
  .app-content {
    min-height: 100vh;
    display: flex;
    flex-direction: column;
  }
</style>