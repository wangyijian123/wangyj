<template>
    <div>
      <SearchBar
        :hot-search="hotSearchKeyword"
        :focus="searchFocus"
        @onChange="onChange"
        @onClear="onClear"
        @onConfirm="onConfirm"
        ref="searchBar"
      />
      <TagGroup
        header-text="热门搜索"
        btn-text="换一批"
        :value="hotSearchArray"
        @onBtnClick="changeHotSearch"
        @onTagClick="showBookDetail"
        v-if="hotSearchArray.length > 0 && !showList"
      />
      <TagGroup
        header-text="历史搜索"
        btn-text="清空"
        :value="historySearch"
        @onBtnClick="clearHistorySearch"
        @onTagClick="searchKeyWord"
        v-if="historySearch.length > 0 && !showList"
      />
      <SearchList
       :data="searchList"
       v-if="showList"
      />
    </div>
</template>
<script>
  import SearchList from '../../components/search/SearchList'
  import SearchBar from '../../components/home/SearchBar'
  import TagGroup from '../../components/base/TagGroup'
  import {getStorageSync, setStorageSync, showToast} from '../../api/wachat'
  import { search, hotSearch } from '../../api'

  const KEY_HISTORY_SEARCH = 'historySearch'

  export default {
    components: {
      TagGroup,
      SearchBar,
      SearchList
    },
    computed: {
      showList() {
        const keys = Object.keys(this.searchList)
        return keys.length > 0
      },
      hotSearchArray() {
        let _hotSearch = []
        this.hotSearch.forEach(o => _hotSearch.push(o.title))
        return _hotSearch
      }
    },
    data() {
      return {
        hotSearch: [],
        hotSearchKeyword: '',
        historySearch: [],
        searchList: {},
        searchFocus: true,
        openId: '',
        page: 1
      }
    },
    methods: {
      onConfirm(keyword) {
        if (!keyword || keyword.trim().length === 0) {
          keyword = this.hotSearchKeyword
          this.$refs.searchBar.setValue(keyword)
        } else {
          return
        }
        if (!this.historySearch.includes(keyword)) {
          this.historySearch.push(keyword)
          setStorageSync(KEY_HISTORY_SEARCH, this.historySearch)
        }
        this.searchFocus = false
        this.onSearch(keyword)
      },
      onClear() {
        this.searchList = {}
      },
      onChange(keyword) {
        if (!keyword || keyword.trim().length === 0) {
          this.searchList = {}
          return
        }
        this.page = 1
        this.onSearch(keyword)
      },
      onSearch(keyword) {
        search({
          keyword,
          openId: this.openId,
          page: this.page
        }).then(response => {
          this.searchList = response.data.data
        })
      },
      changeHotSearch() {
        console.log('change hot search')
      },
      showBookDetail(text, index) {
        console.log('show book detail', text, index)
      },
      clearHistorySearch() {
        this.historySearch = []
        setStorageSync(KEY_HISTORY_SEARCH, [])
      },
      searchKeyWord(text) {
        this.$refs.searchBar.setValue(text)
        this.onSearch(text)
      }
    },
    mounted() {
      this.hotSearchKeyword = this.$route.query.hotSearch
      this.openId = getStorageSync('openId')
      hotSearch().then(response => {
        this.hotSearch = response.data.data
      })
      this.page = 1
      this.historySearch = getStorageSync(KEY_HISTORY_SEARCH) || []
    },
    onPageScroll() {
      if (this.searchFocus) {
        this.searchFocus = false
      }
    },
    onReachBottom() {
      if (this.showList) {
        this.page++
        const searchWord = this.$refs.searchBar.getValue()
        search({
          keyword: searchWord,
          openId: this.openId,
          page: this.page
        }).then(response => {
          const {book} = response.data.data
          console.log(response)
          if (book && book.length > 0) {
            this.searchList.book.push(...book)
          } else {
            showToast('没有更多数据了')
          }
        })
      }
    }
  }
</script>

<style lang="scss" scoped>

</style>
