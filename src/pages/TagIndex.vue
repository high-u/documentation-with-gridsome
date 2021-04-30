<template>
  <Layout>
    <article v-for="tag in $static.tags.edges" :key="tag.id">
      <h2><g-link :to="tag.node.path" rel="bookmark">{{ tag.node.title }}</g-link></h2>
    </article>
  </Layout>
</template>

<static-query>
query Tags {
  tags: allTag (sortBy: "date", order: DESC) {
    edges {
      node {
        id
        title
        path
        belongsTo {
          totalCount
        }
      }
    }
  }
}
</static-query>

