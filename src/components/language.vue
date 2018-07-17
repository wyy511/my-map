<template lang="pug">
section.language-wrap(
  :style="_style"
)
  input(v-for="list in lists" type="button", :value="list.value", :id="list.id", @click="chooseLang(list.id)")
</template>

<script>
export default {
  name: '',
  props: {
    _style: {
      type: Object,
      default: function () {
        return {
          position: 'absolute',
          right: 0,
          top: 0,
          zIndex: 2
        }
      }
    }
  },
  data () {
    return {
      lists: [
        {
          id: 'en',
          value: '显示英文底图'
        },
        {
          id: 'zh_en',
          value: '显示中英文对照底图'
        },
        {
          id: 'zh_cn',
          value: '显示中文底图'
        }
      ]
    }
  },
  methods: {
    chooseLang (id) {
      this.$emit('change-lang', id)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
</style>
