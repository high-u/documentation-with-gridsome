<template>
  <Layout>
    <div class="p-16">
      <div class="mb-12">
      <input type="text" name="search" id="search" placeholder="Type something..." v-model="search" class="rounded-sm bg-hex-212020 h-12 p-4 w-80">
      </div>
      <div v-if="searchResults.length > 0">
        <article v-for="post in searchResults" :key="post.node.id" class="mb-16" >
          <h2 class="mb-4 font-bold text-5xl"><g-link :to="post.node.path">{{ post.node.title }}</g-link></h2>
          <p class="mb-7">{{ post.node.date }}</p>
        </article>
      </div>
      <div v-else>
          <p>Your search didn't return any results. Please try again.</p>
      </div>
    </div>
  </Layout>
</template>

<static-query>
{
  allPost (sortBy: "date", order: DESC) {
    edges {
      node {
        title
        summary
        date
        path
      }
    }
  }
}
</static-query>

<script>
// import Flexsearch from 'flexsearch'
export default {
  data() {
    return {
      search: ''
    }
  },
  computed: {
    searchResults() {
      return this.$static.allPost.edges.filter(post => {
        console.log("this.search.trim()", this.search.trim())
        return this.search.trim() ? post.node.summary.toLowerCase().includes(this.search.toLowerCase().trim()) : false
      }) //.slice(0, 3)
      console.log(hits)

      // highlight
      // if (this.search.length > 0) {
      //   return hits.map(e => {
      //     e.node.highlightSummary = e.node.summary.replaceAll(this.search, `\u{000b}${this.search}\u{000b}`).split("\u{000b}")
      //     return e
      //   })
      // } else {
      //   return hits.map(e => {
      //     e.node.highlightSummary = [e.node.summary]
      //     return e
      //   })
      // }
    }
  }
}
</script>

<style>
.highlight {
  color: white;
  background-color: black;
}
</style>
