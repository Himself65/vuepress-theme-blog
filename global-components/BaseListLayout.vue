<template>
  <div id="base-list-layout">
    <div class="ui-posts">
      <div class="ui-functional">
        <span class="ui-site-title">{{ $site.title }}</span>
        <component v-if="themeToggle" :is="themeToggle"></component>
      </div>

      <div class="ui-post" v-for="page in pages">
        <div class="ui-post-title">
          <NavLink :link="page.path">{{ page.title }}</NavLink>
        </div>

        <div class="ui-post-summary">
          {{ page.frontmatter.summary || page.summary }}
          <!-- <Content :page-key="page.key" slot-key="intro"/>-->
        </div>

        <div class="ui-post-author" v-if="page.frontmatter.author">
          <NavigationIcon/>
          <span>{{ page.frontmatter.author }} in {{ page.frontmatter.location }}</span>
        </div>

        <div class="ui-post-date" v-if="page.frontmatter.date">
          <ClockIcon/>
          <span>{{ resolvePostDate(page.frontmatter.date) }}</span>
        </div>
      </div>
    </div>

    <component v-if="$pagination.length > 1 && paginationComponent" :is="paginationComponent"></component>
  </div>
</template>

<script>
  /* global THEME_BLOG_PAGINATION_COMPONENT */

  import Vue from 'vue'
  import {NavigationIcon, ClockIcon} from 'vue-feather-icons'
  import {Pagination, SimplePagination} from '@vuepress/plugin-blog/lib/client/components'
  import moment from 'moment'

  export default {
    components: {NavigationIcon, ClockIcon},

    data() {
      return {
        paginationComponent: null,
        themeToggle: null
      }
    },

    created() {
      this.paginationComponent = this.getPaginationComponent()
    },

    mounted() {
      import('@theme/components/ThemeToggle.vue').then(module => {
        this.themeToggle = module.default
      })
    },

    computed: {
      pages() {
        return this.$pagination.pages.sort((a, b) => {
          moment(new Date(a.lastUpdated)).isAfter(moment(new Date(b.lastUpdated)))
        })
      },
    },

    methods: {
      getPaginationComponent() {
        const n = THEME_BLOG_PAGINATION_COMPONENT
        if (n === 'Pagination') {
          return Pagination
        }

        if (n === 'SimplePagination') {
          return SimplePagination
        }

        return Vue.component(n) || Pagination
      },

      resolvePostDate(date) {
        return moment(new Date(date)).format(this.$themeConfig.dateFormat || 'ddd MMM DD YYYY')
      }
    }
  }
</script>

<style lang="stylus">
  .common-layout
    .content-wrapper
      padding-bottom 80px

  .ui-functional
    display flex
    flex-direction row
    align-items center
    justify-content space-between
    width 100%
    margin-bottom 2rem

    .ui-site-title
      color var(--title)
      font-size 25px
      font-weight 900

  .ui-post
    padding-bottom 25px
    margin-bottom 25px
    border-bottom 1px solid #f1f1f1

    &:last-child
      border-bottom 0px
      margin-bottom 0px

    p
      margin 0

  .ui-post-title
    font-family PT Serif, Serif
    font-size 28px
    border-bottom 0

    a
      cursor pointer
      color var(--text)
      transition all .2s
      text-decoration none

      &:hover
        text-decoration underline

  .ui-post-summary
    font-size 14px
    margin-bottom 15px
    color var(--text--mask)
    font-weight 200

  .ui-post-author
    display flex
    align-items center
    font-size 12px
    line-height 12px
    color var(--text--mask2)
    margin-bottom 3px
    font-weight 400

    svg
      margin-right 5px
      width 14px
      height 14px

  .ui-post-date
    display flex
    align-items center
    font-size 12px
    color var(--text--mask)
    font-weight 200

    svg
      margin-right 5px
      width 14px
      height 14px
</style>

<style src="prismjs/themes/prism-okaidia.css"></style>


