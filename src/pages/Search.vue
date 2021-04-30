<template>
  <Layout>
    <input type="text" name="search" id="search" placeholder="Type something..." v-model="search">

    <div v-if="searchResults.length > 0">
      <article v-for="post in searchResults" :key="post.node.id">
        <h2><g-link :to="post.node.path">{{ post.node.title }}</g-link></h2>
        <p>{{ post.node.date }}</p>

        <span
          v-for="(words, index) in post.node.highlightSummary" :key="index"
          :class="{'highlight': words == search}"
          >
          {{ words }}
        </span>

      </article>
    </div>

    <div v-else>
        <p>Your search didn't return any results. Please try again.</p>
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
      const hits = this.$static.allPost.edges.filter(post => {
        return post.node.summary.toLowerCase().includes(this.search.toLowerCase().trim())
      }).slice(0, 3)
      console.log(hits)
      if (this.search.length > 0) {
        return hits.map(e => {
          e.node.highlightSummary = e.node.summary.replaceAll(this.search, `\u{000b}${this.search}\u{000b}`).split("\u{000b}")
          return e
        })
      } else {
        return hits.map(e => {
          e.node.highlightSummary = [e.node.summary]
          return e
        })
      }
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
