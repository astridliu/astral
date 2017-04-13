<template>
  <div class="tag-editor dropdown">
    <vue-tagger :tags="tags" @change="tagsChanged" :placeholder="'Add a tag'"></vue-tagger>
    <button type="button" name="button" class="tag-editor--save-tags btn-flat" @click="syncTags">Save Tags</button>
  </div>
</template>
<script>
import Vue from 'vue'
import VueTagger from 'vue-tagger'

export default {
  name: 'TagEditor',
  props: ['tags'],
  components: {
    VueTagger
  },
  data () {
    return {
      tagsToSync: []
    }
  },
  created () {
    this.tagsToSync = this.tags.map(function (tag) {
      return {
        name: tag.name,
        selected: tag.selected
      }
    }).filter(function (tag) {
      return tag.selected
    })

    this.$bus.$on('CURRENT_TAGS_CHANGED', (tags) => { this.tagsToSync = tags })
  },
  methods: {
    syncTags () {
      this.$bus.$emit('SYNC_TAGS', this.tagsToSync)
    },
    tagsChanged (tags) {
      const tagData = tags.map((tag) => {
        return {
          name: tag,
          selected: true
        }
      })
      this.$bus.$emit('CURRENT_TAGS_CHANGED', tagData)
    }
  }
}
</script>
