<template>
  <div>
    <HomeHero :page="page" />
    <Security />
    <HomeAbout :page="page" />
    <HomeBlog :posts="postsResults" />
    <HomeExperience :page="page" />
    <SVGSwoosh />
    <HomeBackers :page="page" />
    <SVGAdoraWink />
  </div>
</template>

<script>
import Security from '~/components/Home/Security.vue'
import postsQuery from '~/apollo/queries/post/posts'

export default {
  data() {
    return {
      postsResults: [],
    }
  },
  async asyncData({ $content, $graphql }) {
    const [page, postsData] = await Promise.all([
      $content('pages/home').fetch(),
      $graphql.default.request(postsQuery, {
        page: 1,
        pageSize: 3,
      }),
    ])

    return {
      page: page,
      postsResults: postsData.posts.data,
    }
  },
  components: { Security },
}
</script>
