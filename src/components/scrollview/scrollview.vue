<template>
    <scroll-view scroll-x="true" class="header-nav"  >
        <div class="nav-list">
            <div class="nav-item" :class="{'active': changeNav == index}"
                v-for="(item,index) in navs" :key="index" :data-nav="index" @click="chooseTitle(index)">
                {{item}}
            </div>   
        </div>
    </scroll-view>
</template>

<script>
export default {
    props: {
        navs: Array
    },
    data() {
        return {
            changeNav: 0
        }  
    },
    methods: {
        chooseTitle(index) {
            // console.log(index)
            this.changeNav = index;
            // wx.navigateTo({
            //     url: ''
            // })
        }
    }
}
</script>

<style lang="stylus" scoped>
  .header-nav 
    width 600rpx
    height 80rpx
    color red
    overflow scroll
    .nav-list
        width auto
        height 80rpx
        white-space nowrap
        .nav-item
            text-align center
            line-height 80rpx
            color #ffffff
            font-size 30rpx
            margin-right 20rpx
            width 100rpx
            height 80rpx
            display inline-block
        .active
            font-size 33rpx
            font-weight bold
</style>
