<template>
  <div class="search-bar">
    <div class="search-bar-wrapper" @click="onSearchBarClick">
      <van-icon name="search" class="search" size="16px" color="#858c96"></van-icon>
      <input type="text"
             :focus="focus"
             :disabled="disabled"
             :maxlength="limit"
             :placeholder="hotSearch.length === 0 ? '搜索' : hotSearch"
             v-model="searchWord"
             @input="onChange"
             confirm-type="search"
             placeholder-style="color: #ADB4BE:font-size:15px"
             @confirm="onConfirm"
             class="search-bar-input">
      <van-icon
        name="clear"
        class="clear"
        size="16px"
        color="#cccccc"
        @click="onClearClick"
        v-if="searchWord.length>0"
      ></van-icon>
    </div>
  </div>
</template>
<script>
    export default {
      props: {
        focus: {
          type: Boolean,
          default: false
        },
        disabled: {
          type: Boolean,
          default: false
        },
        limit: {
          type: Number,
          default: 10
        },
        hotSearch: {
          type: String,
          default: ''
        }
      },
      data() {
        return {
          searchWord: ''
        }
      },
      methods: {
        onSearchBarClick() {
          this.$emit('onClick')
        },
        onClearClick() {
          this.searchWord = ''
          this.$emit('onClear')
        },
        onChange(e) {
          const { value } = e.mp.detail
          this.$emit('onChange', value)
        },
        onConfirm(e) {
          const { value } = e.mp.detail
          this.$emit('onConfirm', value)
        },
        setValue(v) {
          this.searchWord = v
        },
        getValue() {
          return this.searchWord
        }
      }
    }
</script>

<style lang="scss" scoped>
  .search-bar{
    padding: 15px 15.5px;
    .search-bar-wrapper{
      display: flex;
      align-items: center;
      background: #f5f7f9;
      border-radius: 20px;
      height: 40px;
      padding: 12px 17px;
      box-sizing: border-box;
      .search,.clear{
        display: flex;
        align-items: center;
        height: 100%;
      }
      .search-bar-input{
        flex: 1;
        margin:0 8px;
        font-size: 15px;
        color: #333;
      }
    }
  }
</style>
