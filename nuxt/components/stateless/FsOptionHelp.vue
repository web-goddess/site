<template>
  <div class="py-5">
    <fs-docs-pattern-option-image :pattern="pattern" :option="option" />
    <div v-html="content.body"></div>
  </div>
</template>

<script>
import FsDocsPatternOptionImage from '~/components/stateless/FsDocsPatternOptionImage'

export default {
  name: 'FsOptionHelp',
  components: {
    FsDocsPatternOptionImage
  },
  props: {
    pattern: {
      type: String,
      required: true
    },
    option: {
      type: String,
      required: true
    },
  },
  data: function() {
    return {
      content: ''
    }
  },
  mounted: async function() {
    let content = {}
    let loc = this.$fs.pathLocale(this.$route.path)
    let locPath = (loc === 'en') ? '' : '/'+loc
    let path = locPath+'/docs/patterns/'+this.pattern.toLowerCase()+'/options/'+this.option.toLowerCase()
    content = await this.$fs.content('/'+this.$i18n.locale+'/docs').get(path)
    .then(function (data) {
      return data
    })
    .catch(function (res) {
      console.log('Content not found')
    })
    if((typeof content === 'undefined' || typeof content.body === 'undefined') && this.$i18n.locale !== 'en') {
      console.log('Trying English version')
      content = await this.$fs.content('/en/docs').get(path.substr(3))
      .then(function (data) {
        return data
      })
    }
    this.content = content
  },
}
</script>
