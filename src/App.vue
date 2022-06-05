<template>
  <div id="app">
    <main>
      <div class="search-box">
        <input 
          type="text" 
          class="search-bar" 
          placeholder="Search..."
          />
      </div>
    </main>
  </div>
</template>
<script>
export default {
  name: 'app',
  data() {
    return {
      USPLS_ACCESS_KEY: 'XjEXS7Bgzz23z4VX3ldbgTjxLjTCrNLijrt1I8Pz5ho',
      USPLS_URL: 'https://api.unsplash.com/',
      
    }
  },

}
</script>
<style scoped >
  
</style>