<template lang="pug">
v-row(tag='section', no-gutters)
  v-col(cols='12', md='10', lg='8', offset-md='1', offset-lg='2')
    v-row(justify='center')
      v-col(
        cols='12',
        sm='6',
        lg='6',
        v-for='federation in content',
        :key='federation.path'
      )
        preview-card(:content='federation')
</template>

<script>
export default {
  async asyncData({ $content }) {
    const data = await $content('federation', { deep: true })
      .only(['title', 'path', 'description', 'color', 'order'])
      .where({ extension: '.md', slug: 'index' })
      .sortBy('order')
      .fetch()

    data.map((obj) => {
      // search for the 'index term'
      const place = obj.path.lastIndexOf('/index')

      if (place === -1) {
        return obj
      }

      // Remove term for the path
      obj.path = obj.path.slice(0, place)
      return obj
    })

    // Remove deeper path (more than the federation )
    const content = data.filter(
      (obj) => [...obj.path.matchAll(/\//g)].length <= 2
    )

    // const content = await $content('federation', 'index').fetch()
    return {
      content,
    }
  },
  head() {
    return {
      title: 'fédération'.toUpperCase(),
      meta: [
        {
          hid: 'description',
          name: 'description',
          content:
            "Retrouver l'ensemble des pôles du Campus de l'INSA Centre-Val de Loire",
        },
        {
          hid: 'og:title',
          property: 'og:title',
          content: `FÉDÉRATION - Campus INSA Centre-Val de Loire`,
        },
        {
          hid: 'og:description',
          property: 'og:description',
          content:
            "Retrouver l'ensemble des pôles du Campus de l'INSA Centre-Val de Loire",
        },
        {
          hid: 'twitter:title',
          name: 'twitter:title',
          content: `FÉDÉRATION - Campus INSA Centre-Val de Loire`,
        },
        {
          hid: 'twitter:description',
          name: 'twitter:description',
          content:
            "Retrouver l'ensemble des pôles du Campus de l'INSA Centre-Val de Loire",
        },
      ],
    }
  },
}
</script>

<style scoped>
a {
  transition: box-shadow 0.2s ease-in-out;
}
</style>
