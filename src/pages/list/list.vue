<template>
    <div>
      <SearchTable :data="data" />
    </div>
</template>
<script>
  import { searchList } from '../../api'
  import { setNavigationBarTitle } from '../../api/wachat'
  import SearchTable from '../../components/search/SearchTable'

  export default {
    components: {SearchTable},
    data() {
      return {
        data: []
      }
    },
    methods: {
      getSearchList() {
        const { key, text } = this.$route.query
        const params = {}
        if (key && text) {
          params[key] = text
        }
        searchList(params).then(response => {
          this.data = response.data.data
        })
      }
    },
    mounted() {
      this.getSearchList()
      const { title } = this.$route.query
      setNavigationBarTitle(title)
    }
  }
</script>

<style lang="scss" scoped>

</style>
