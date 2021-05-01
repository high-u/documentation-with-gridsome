<template>
  <Layout>
    <a :href="filepath">Edit</a>
    <article>
      <h1>{{ $page.post.title }} </h1>

      <div>
        Tags:
        <g-link
            v-for="tag in $page.post.tags"
            :to="tag.path"
            :key="tag.id">
          #{{ tag.title }}
        </g-link>
      </div>

      <p>Posted on {{ $page.post.date }}</p>

      <div class="markdown-body mb-8" id="article-area" v-html="contentWithTailwind" />
    </article>
  </Layout>
</template>

<page-query>
query Post ($path: String!) {
  post: post (path: $path) {
    title
    date (format: "MMMM D, Y")
    content
    tags {
      title
      path
    }
    fileInfo {
      path
    }
  }
}
</page-query>

<script>
  export default {
  metaInfo() {
    return {
      title: this.$page.post.title
    }
  },
  computed: {
    filepath: function() {
      return `https://github.com/high-u/documentation-with-gridsome/blob/main/${this.$page.post.fileInfo.path}`
    },
    contentWithTailwind() {
      let domparser = new DOMParser()
      let doc = domparser.parseFromString(this.$page.post.content, "text/html")
      // console.log(doc)
      // doc.querySelectorAll(".xxx")
      // let pres = doc.getElementsByTagName("pre")
      // console.log(pres)
      // let button = document.createElement("button")
      // let appendedButton = pres[0].appendChild(button)
      // console.log("pres[0]", pres[0])
      // console.log("appendedButton", appendedButton)

      let h2 = doc.getElementsByTagName("h2")
      for (let e of h2) {
        e.classList.add("text-yellow-500", "underline")
      }


      // console.log(doc.documentElement.innerHTML)
      return doc.documentElement.innerHTML
    }
  }
}
</script>

