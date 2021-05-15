<template>
  <nav class="overflow-y-auto overflow-x-hidden ">
    <div v-for="(posts, category) in categoryTree" :key="category" >
      <div class="text-sm text-gray-400 opacity-50 px-2 pt-5 pb-2">{{ category }}</div>
      <ul>
        <li v-for="post in posts" :key="post.id">
          <g-link :to="post.path" class="text-gray-400 leading-tight inline-block my-1 py-1 px-2 hover:bg-hex-292a2d rounded">{{ post.title }}</g-link>
        </li>
      </ul>
    </div>
  </nav>
</template>

<static-query>
query Categories {
  posts: allPost (sortBy: "date", order: ASC, filter: {categories: {id: {exists:true}}}){
    edges {
      node {
        id
        title
        path
        categories {
          title
        }
      }
    }
  }
}
</static-query>

<script>

export default ({
  computed: {
    categoryTree() {
      let tree = [
        {
          title: "abc",
          path: "/category/abc",
          children: [
            {
              title: "def",
              path: "/category/abc%2Fdef",
              children: [
                {
                  title: "ghi",
                  path: "/category/abc%2Fdef%2Fghi",
                }
              ]
            }
          ]
        }
      ]
      tree = {}
      for (let e of this.$static.posts.edges) {
        // console.log(e.node.categories.title)
        const a = {
          title: e.node.title,
          path: e.node.path
        }
        if (tree[e.node.categories.title]) {
          tree[e.node.categories.title].push(a)
        } else {
          tree[e.node.categories.title] = [a]
        }
      }
      // return this.$static.category.edges
      console.log(tree)
      return tree
    }
  }
})
</script>
