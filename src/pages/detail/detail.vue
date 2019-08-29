<template>
    <div>
      <DetailBook :book="book" />
<!--      <DetailStat-->
<!--        :readers="book.readers"-->
<!--        :reader-num="book.readerNum"-->
<!--        :rank-num="book.rankNum"-->
<!--        :rank-avg="book.rankAvg"-->
<!--      />-->
<!--      <DetailRate-->
<!--        :rate-value="book.rateValue"-->
<!--        @onRateChange="onRateChange"-->
<!--      />-->
      <DetailContents
        :contents="contents"
        @readBook="readBook"
      />
      <DetailBottom
        :is-in-shelf="isInShelf"
        @handleShelf="handleShelf"
        @readBook="readBook"
      />
    </div>
</template>
<script>
  import DetailBook from '../../components/detail/DetailBook'
  import DetailStat from '../../components/detail/DetailStat'
  import DetailRate from '../../components/detail/DetailRate'
  import DetailContents from '../../components/detail/DetailContents'
  import DetailBottom from '../../components/detail/DetailBottom'
  import {
    bookDetail,
    bookRankSave,
    bookContents,
    bookIsInShelf,
    bookShelfSave,
    bookShelfRemove
  } from '../../api'
  import { getStorageSync } from '../../api/wachat'

  export default {
    components: { DetailBottom, DetailContents, DetailRate, DetailStat, DetailBook },
    data() {
      return {
        book: {},
        contents: [],
        isInShelf: false
      }
    },
    mounted() {
      this.getBookDetail()
      this.getBookContents()
      this.getBookIsInShelf()
    },
    methods: {
      onRateChange(value) {
        const openId = getStorageSync('openId')
        const { fileName } = this.$route.query
        bookRankSave({openId, fileName, rank: value}).then(response => {
          this.getBookDetail()
        })
      },
      getBookDetail() {
        const openId = getStorageSync('openId')
        const {fileName} = this.$route.query
        if (openId && fileName) {
          bookDetail({openId, fileName}).then(response => {
            this.book = response.data.data
          })
        }
      },
      getBookContents() {
        const { fileName } = this.$route.query
        if (fileName) {
          bookContents({fileName}).then(response => {
            this.contents = response.data.data
          })
        }
      },
      readBook(href) {
        console.log(href)
      },
      getBookIsInShelf() {
        const openId = getStorageSync('openId')
        const { fileName } = this.$route.query
        bookIsInShelf({openId, fileName}).then(response => {
          const {data} = response.data
          data.length === 0 ? this.isInShelf = false : this.isInShelf = true
        })
      },
      handleShelf() {
        const openId = getStorageSync('openId')
        const {fileName} = this.$route.query
        if (!this.isInShelf) {
          bookShelfSave({openId, fileName}).then(this.getBookIsInShelf)
        } else {
          const vue = this
          mpvue.showModal({
            title: '提示',
            content: `是否确认将《${this.book.title}》移出书架`,
            success (res) {
              if (res.confirm) {
                bookShelfRemove({openId, fileName}).then(vue.getBookIsInShelf)
              }
            }
          })
        }
      }
    }
  }
</script>

<style lang="scss" scoped>

</style>
