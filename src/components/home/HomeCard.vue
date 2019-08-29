<template>
  <div class="home-card">
    <div class="home-card-inner">
      <div class="user-info">
        <div class="avatar-wrapper">
          <ImageView
            :src="avatar"
            height="100%"
            mode="scaleToFill"
            round
          />
        </div>
        <div class="nickname">{{nikename}}</div>
        <div class="shelf-text">书架共有{{num}}本好书</div>
        <div class="round-item"></div>
        <div class="shelf-text">特别精选</div>
      </div>
      <div class="book-info">
        <div class="book-wrapper">
          <div class="book-img-wrapper"
               @click="onBookClick(item)"
               v-for="(item,index) in bookList"
               :key="index">
            <ImageView
              :src="item.cover"
            />
          </div>
        </div>
        <div class="shelf-wrapper">
          <div class="shelf">书架</div>
          <van-icon class="arrow" name="arrow" size="11px" color="#828489"></van-icon>
        </div>
      </div>
      <div class="feedback-wrapper" @click="onFeedBackClick">反馈</div>
    </div>
    <van-dialog id="van-dialog" />
  </div>
</template>
<script>
  import ImageView from '../base/ImageView'
  import Dialog from 'vant-weapp/dist/dialog/dialog'

  export default {
    components: { ImageView },
    props: {
      data: Object,
      hasSign: {
        type: Boolean,
        default: false
      },
      signDay: {
        type: Number,
        default: 0
      }
    },
    computed: {
      avatar() {
        return (this.data && this.data.userInfo && this.data.userInfo.avatarUrl) || ''
      },
      nikename() {
        return (this.data && this.data.userInfo && this.data.userInfo.nickName) || ''
      },
      num() {
        return (this.data && this.data.num) || 0
      },
      bookList() {
        return (this.data && this.data.bookList) || []
      }
    },
    methods: {
      gotoShelf() {},
      onBookClick(item) {
        this.$emit('onclick', item)
      },
      sign() {},
      onFeedBackClick() {
        Dialog.confirm({
          title: '反馈',
          message: '您是否确认提交反馈信息？',
          confirmButtonText: '是',
          cancelButtonText: '否'
        }).then(() => {
          console.log('点击是之后的事件')
        }).catch(() => {
          console.log('点击否之后的事件')
        })
      }
    }
  }
</script>

<style lang="scss" scoped>
  .home-card{
    background-image: linear-gradient(-90deg, #54575f 0%, #595b60 100%);
    border-radius: 15px;
    margin: 22px 20px 0;
    .home-card-inner{
      padding: 21.5px 17px 20px 20px;
      box-sizing: border-box;
      position: relative;
      .user-info{
        display: flex;
        align-items: center;
        .avatar-wrapper{
          width: 20px;
          height: 20px;
        }
        .nickname,.shelf-text{
          font-size: 12px;
          color: #fff;
        }
        .nickname{
          margin:0 8.5px;
        }
        .shelf-text{
          opacity: 0.7;
        }
        .round-item{
          width: 4px;
          height: 4px;
          background: #a2a2a2;
          border-radius: 50%;
          margin: 0 8px;
        }
      }
      .book-info{
        display: flex;
        margin-top: 16.5px;
        .book-wrapper{
          display: flex;
          justify-content: space-around;
          flex: 1;
          .book-img-wrapper{
            width: 72px;
            height: 101px;
          }
        }
        .shelf-wrapper{
          display: flex;
          align-items: center;
          .shelf{
            width: 11px;
            font-size: 11px;
            word-break: break-word;
            color: #fff;
            opacity: 0.8;
          }
        }
      }
      .feedback-wrapper{
        position: absolute;
        right: 0;
        top: 19.5px;
        width: 44.5px;
        height: 23.5px;
        line-height: 23.5px;
        text-align: center;
        background: #707070;
        /*opacity: 0.3;*/
        border-radius: 100px 0 0 100px;
        font-size: 11px;
        color: #fff;
      }
    }
  }
</style>
