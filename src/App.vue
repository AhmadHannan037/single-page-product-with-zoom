<template>
  <div id="app" class="relative min-h-screen pb-64 antialiased bg-gray-100">
    <header>
      <HeaderBar />
    </header>
    <main class="max-w-6xl mx-auto lg:mt-0">
      <ProductPage />
    </main>

  </div>
</template>

<script>
import HeaderBar from './components/utils/HeaderBar.vue'
import ProductPage from './views/ProductPage.vue'
export default {
  name: 'App',
  components: {
    HeaderBar,
    ProductPage
  }
}
</script>

<style>
html,body{
  margin: 0%;
  padding: 0%;
  width: 100%;
  overflow-x:hidden ;
}

</style>
