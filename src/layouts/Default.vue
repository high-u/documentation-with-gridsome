<template>
  <div class="flex min-h-screen bg-gray-900">
    <div class="flex-none w-72 bg-gray-800">
      <navbar />
    </div>
    <div class="flex-grow overflow-y-auto overflow-x-hidden">
    
      <header class="">
        <strong>
          <g-link to="/">{{ $static.metadata.siteName }}</g-link>
        </strong>
        <nav class="nav">
          <g-link class="nav__link" to="/">Home</g-link>
          <g-link class="nav__link" to="/blog/">Blog</g-link>
          <g-link class="nav__link" to="/search/">Search</g-link>
          <g-link class="nav__link" to="/tag-index/">Tags</g-link>
          <g-link class="nav__link" to="/about/">About</g-link>
        </nav>
      </header>
      <slot/>

    </div>
  </div>
</template>

<static-query>
query {
  metadata {
    siteName
  }
}
</static-query>

<script>
import navbar from "~/components/navbar";
// import Navbar from '../components/navbar.vue';
export default {
  components: {
    navbar
  }
};
</script>

<style>
body {
  font-family: -apple-system,system-ui,BlinkMacSystemFont,"Segoe UI",Roboto,"Helvetica Neue",Arial,sans-serif;
  margin:0;
  padding:0;
  line-height: 1.5;
}

.layout {
  max-width: 760px;
  margin: 0 auto;
  padding-left: 20px;
  padding-right: 20px;
}

.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
  height: 80px;
}

.nav__link {
  margin-left: 20px;
}
</style>
