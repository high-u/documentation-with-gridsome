<template>
  <Layout>
    <input type="text" name="search" id="search" placeholder="Type something..." v-model="search">

    <div v-if="searchResults.length > 0">
      <article v-for="post in searchResults" :key="post.node.id">
        <h2><g-link :to="post.node.path">{{ post.node.title }}</g-link></h2>
        <p>{{ post.node.date }}</p>
        <p>{{ post.node.summary }}</p>
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
      return this.$static.allPost.edges.filter(post => {
        return post.node.summary.toLowerCase().includes(this.search.toLowerCase().trim())
      }).slice(0, 3)
    }
  }
}
</script>

