<template>
  <Layout>
    <div class="p-16">
    <div class="grid justify-items-end mb-6"><a :href="filepath" class="text-sm underline">Edit</a></div>
    <article class="">
      <h1 class="mb-4 font-bold text-5xl">{{ $page.post.title }} </h1>

      <div class="mb-2 opacity-50">
        Tags:
        <g-link
            v-for="tag in $page.post.tags"
            :to="tag.path"
            :key="tag.id">
          #{{ tag.title }}
        </g-link>
      </div>

      <p class="mb-7">Posted on {{ $page.post.date }}</p>

      <div class="markdown-body" id="article-area" v-html="contentWithTailwind" />
    </article>
    </div>
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

      const classes = {
        "p": ["my-4"],
        "a": ["underline"],
        "h1": ["text-4xl", "font-bold", "my-6"],
        "h2": ["text-3xl", "font-bold", "my-5"],
        "h3": ["text-2xl", "font-bold", "my-4"],
        "h4": ["text-xl", "font-bold", "my-4"],
        "h5": ["text-lx", "font-bold", "my-4"],
        "h6": ["text-lx", "my-4"],
        "table": ["border", "border-collapse", "text-left", "w-full", "border-gray-600", "p-2", "my-10"],
        "th": ["border", "border-hex-4a4b50", "bg-hex-3b3d42", "p-2"],
        "td": ["border", "border-gray-600", "p-2"],
        "ul": ["my-4", "ml-10", "list-disc"],
        "ol": ["my-4", "ml-10", "p-0", "list-decimal"],
        "ul li ul": ["my-0", "ml-10", "list-disc"],
        "ol li ol": ["my-0", "ml-10",, "p-0", "list-decimal"],
        "li": ["m-1"],
        "li p": ["my-0"],
        "blockquote": ["border-l-2", "ml-10", "px-6", "py-3"],
        "blockquote p": ["ml-0"],
        "code": ["p-1", "bg-hex-212020", "rounded"],
        "pre": ["p-4", "bg-hex-212020", "rounded"],
        "pre code": ["p-0", "rounded-none"],
        "hr": ["border-t-1", "border-gray-600"],

  
      }
      for (let [tag, classList] of Object.entries(classes)) {
        const family = tag.split(" ")
        console.log("family", family)
        let elem = []
        elem.push(doc)
        for (let f of family) {
          let children = []
          for (let e of elem) {
            let aaa = e.getElementsByTagName(f)
            for (let a of aaa) {
              children.push(a)
            }
          }
          elem = children
        }
        for (let e of elem) {
          // e.classList.add(...classList)
          e.classList = classList.join(" ")
        }
      }
      // console.log(doc.documentElement.innerHTML)
      return doc.documentElement.innerHTML
    }
  }
}
</script>


