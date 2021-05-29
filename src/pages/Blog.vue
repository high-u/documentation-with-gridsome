<template>
  <Layout>
    <div class="p-16">
    <article v-for="post in $page.posts.edges" :key="post.id" class="mb-16" >
      <h2 class="mb-4 font-bold text-5xl"><g-link :to="post.node.path" rel="bookmark">{{ post.node.title }}</g-link></h2>
      <p class="mb-7">Posted on <time :datetime="post.node.date">{{ post.node.date }}</time></p>

      <p>{{ post.node.summary }}</p>
    </article>


    <h2>Pagination</h2>
    <Pager :info="$page.posts.pageInfo"/>
    </div>
  </Layout>
</template>

<page-query>
query Posts ($page: Int) {
  posts: allPost (sortBy: "date", order: DESC, perPage: 3, page: $page) @paginate {
    totalCount
    pageInfo {
      totalPages
      currentPage
    }
    edges {
      node {
        id
        title
        date (format: "MMMM D, Y")
        summary
        path
      }
    }
  }
}
</page-query>

<script>
import { Pager } from 'gridsome'

export default {
  components: {
    Pager
  },
  metaInfo: {
    title: 'View my blog posts'
  },
}
</script>

