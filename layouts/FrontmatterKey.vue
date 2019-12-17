<template>
  <div id="vuperess-theme-blog__tags-layout">
    <BlogTags :tags="$frontmatterKey.list"/>
  </div>
</template>

<script>
  export default {
    mounted() {
      import('@theme/components/util').then(module => {
        module.initTheme()
      })
    }
  }
</script>
