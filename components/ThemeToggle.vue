<template>
  <ToggleButton
    :labels="{ checked: 'Dark', unchecked: 'Light' }"
    :value="this.defaultValue"
    :width="55"
    @change="onChange"
    color="#8a278c"
  />
</template>

<script>
  import {ToggleButton} from 'vue-js-toggle-button'
  import {darkThemeKey} from './util'

  export default {
    components: {ToggleButton},
    data() {
      return {
        defaultValue: false
      }
    },
    beforeMount() {
      this.defaultValue = window.localStorage.getItem(darkThemeKey) === 'true' || false
    },
    methods: {
      onChange({value}) {
        window.localStorage.setItem(darkThemeKey, value)
        this.setTheme()
      },

      setTheme() {
        import('@theme/components/util').then(module => {
          module.initTheme()
        })
      }
    }
  }
</script>

<style scoped>

</style>
