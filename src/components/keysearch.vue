<template lang="pug">
section.keysearch-wrap
  input#keysearch(
    type="text",
    v-model="val", 
    @input="keysearch"
  )
  ul.search-lists
    li.list(
      v-for="(item, index) in list",
      :key="index"
    )
</template>

<script>
export default {
  name: '',
  props: {
    value: String,
    list: Array
  },
  data () {
    return {
    }
  },
  created() {
    this.val = this.value
  },
  methods: {
    keysearch () {
      this.$emit('search', {
        input: 'keysearch',
        val: this.val
      })
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
</style>
