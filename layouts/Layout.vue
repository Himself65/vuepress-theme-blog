<template>
  <div>
    <BaseListLayout v-if="$pagination"/>
    <Content v-else/>
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

<style src="prismjs/themes/prism-okaidia.css"></style>
