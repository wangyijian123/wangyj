<template>
    <div class="auth-wrapper">
      <div class="auth">
        <div class="auth-info">
          <div class="auth-img">
            <ImageView
              height="100%"
              mode="scaleToFill"
              src="https://www.youbaobao.xyz/mpvue-res/logo.jpg"
              round
            />
          </div>
          <div class="sub-title">登录小慕读书</div>
          <div class="title">全球好书免费阅读</div>
        </div>
        <button
          open-type="getUserInfo"
          @getuserinfo="getUserInfo"
          class="auth-btn"
        >授权登录</button>
      </div>
    </div>
</template>
<script>
  import ImageView from './ImageView'
  export default {
    components: {
      ImageView
    },
    methods: {
      getUserInfo() {
        this.$emit('getUserInfo')
      }
    }
  }
</script>

<style lang="scss" scoped>
  .auth-wrapper{
    position: fixed;
    left: 0;
    top: 0;
    background: #999;
    width: 100%;
    height: 100%;
    z-index: 1000;
    display: flex;
    justify-content: center;
    align-items: center;
    .auth{
      position: relative;
      width: 270px;
      height: 248px;
      background: #f5f5f5;
      border-radius: 18px;
      .auth-info{
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        margin-top: 22.5px;
        .auth-img{
          width: 74px;
          height: 74px;
        }
        .sub-title{
          margin-top: 18px;
          line-height: 18.5px;
          font-size: 13px;
          color: #999;
        }
        .title{
          margin-top: 3.5px;
          line-height: 22.5px;
          font-size: 16.5px;
          font-weight: 400;
          color: #333;
        }
      }
      .auth-btn{
        position: absolute;
        left: 0;
        bottom: 0;
        width: 100%;
        height: 49px;
        line-height: 49px;
        background-image: linear-gradient(90deg, #1ea3f5 0%, #0f87fc 100%);
        border-radius: 0 0 18px 18px;
        color: #fff;
        font-size: 15px;
      }
    }
  }
</style>
