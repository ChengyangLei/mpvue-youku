<template>
    <div class="discovery">
        <div class="play-video" >
            <video-play></video-play>
        </div>
        <div class="play-container" v-if="isShowAllDesc">
            <div class="video-desc line">
                <div class="video-title">
                    <div class="video" :class="{actived: isActived1}" @click="changeTopic1()">视频</div>
                    <div class="star" :class="{actived: isActived2}" @click="changeTopic2()">星球</div>
                </div>
                <div v-if="chooseTopic">
                    <div class="video-content" >
                        <weui-img imgIcon="../../../static/images/yk-logo-1.png" titleText="古剑奇谭2" isShowTo="true" toText="简介"></weui-img>
                        <div class="video-title-desc">
                            <span class="title-desc-rating">{{play.rating}}</span><span>/{{play.playKind}}/{{play.playTotal}}/{{play.playNum}}</span>
                        </div>
                        <div class="video-content-desc">
                            <span>{{play.desc}}</span>
                        </div>
                    </div>
                    <div class="play-vip">
                        <weui-no titleText="选集" v-on:navigateColl="navigateColl" isShowTo="true" contentText="会员周二更新好多，非会员周一更新"></weui-no>
                        <div class="vip-choose">
                            <scroll-box></scroll-box>
                        </div>    
                    </div>
                    <div class="plays-around">
                        <div class="around-title">
                            <weui-no titleText="周边视频" isShowTo="true"></weui-no>
                        </div>
                        <div class="around-video">
                            <scroll-video></scroll-video>
                        </div>
                    </div>
                    <div class="plays-holiday">
                        <div class="around-title">
                            <weui-no titleText="必看，暑假好剧都在这里" isShowTo="true" contentText="全部"></weui-no>
                        </div>
                        <div class="around-video">
                            <scroll-video></scroll-video>
                        </div>
                    </div>
                </div>
                <scroll-view scroll-y style="height:500px" class="comment" v-if="!chooseTopic">
                    <div  v-for="(comment, index) in comments" :key="index" >
                        <comment :comment="comment"></comment>
                    </div>
                </scroll-view>
            </div>
        </div>
        <div class="plays-choose" v-if="isShowPlays" :animation="animationData">
            <play-box v-on:hideAnthology="hideAnthology"></play-box>
        </div>
    </div>
</template>

<script>
import VideoPlay from '@/components/VideoPlay/VideoPlay'
import weuiImg from '@/components/title/weuiImg'
import weuiNo from '@/components/title/weuiNo'
import scrollBox from '@/components/scrollView/scrollBox'
import playBox from './playBox'
import scrollVideo from '@/components/scrollView/scrollVideo'
import Comment from '@/components/comment/Comment'
import fly from '@/utils/fly'
import CODE_STATUS from '@/utils/config'
export default {
    data () {
        return {
            chooseTopic: true,
            isActived1: true,
            isActived2: false,
            play: '',
            isShowAllDesc: true,//除视频以下
            isShowV: false,
            animationData: {},
            isShowPlays: false,// 选集
            comments: []
        }   
    },
    components: {
        VideoPlay,
        weuiImg,
        weuiNo,
        scrollBox,
        playBox,
        scrollVideo,
        Comment
    },
    methods: {
        navigateColl () {
            const animationShowHeight = 350
             var animation = wx.createAnimation({
            duration: 150,
            timingFunction: "linear",
            delay: 0
        })
            animation= animation.translateY(animationShowHeight).step()
            this.animationData = animation.export(),
            this.isShowPlays = true
            setTimeout(function () {
                animation.translateY(0).step()
                this.animationData=animation.export()
                this.isShowAllDesc = false
                }.bind(this), 150)
            }, 
        hideAnthology () {
            const animationShowHeight = 350
             var animation = wx.createAnimation({
            duration: 150,
            timingFunction: "linear",
            delay: 0
        })
            animation= animation.translateY(animationShowHeight).step()
            this.animationData = animation.export(),
            this.isShowAllDesc = true
            setTimeout(function () {
                animation.translateY(0).step()
                this.animationData=animation.export()
                this.isShowPlays = false
            }.bind(this), 150)
        },
        changeTopic1() {
            this.isActived1 = true
            this.isActived2 = false
            this.chooseTopic = true
        },
        changeTopic2() {
            this.isActived1 = false
            this.isActived2 = true
            this.chooseTopic = false
        },
        // 请求页面数据
        getPlay(id) {
            wx.showLoading({
            title: '加载中',
        })
            this.$http.get('play/2001')// 数据接口只有2001
            .then((res) => {
                if (res.status = CODE_STATUS) {
                this.comments = res.data.data.comment;
                this.play = res.data.data.play
                wx.hideLoading()
                }
            })
       }         
    },
    onLoad(options) {
        const id = options.id
        this.getPlay(id)
    }
}
</script>

<style lang="stylus" scoped>
@import  '../../common/style/mixin'
    .play-video
        height 400rpx

    .video-title
        wh(250rpx, 75rpx)
        margin 0 auto
        flex(space-between)
        font-size 30rpx
        color #222222
        position relative
        .actived
            font-weight bold
            font-size 32rpx
            position relative
            &::after
                content ''
                wh(65rpx, 1rpx)
                background-color #1296db
                position absolute
                left -0rpx
                z-index 200
                top 55rpx
        &::after
            content ''
            wh(750rpx, 1rpx)
            background-color #bfbfbf
            position absolute
            left -250rpx
            z-index 200
            top 70rpx
            transform scaleY(0.4)
    
    .video-content
        padding-left 10rpx
        padding-bottom 0
    .video-title-desc
        font-size 25rpx
        color #bfbfbf
        hlh(40rpx)
        padding-left 10rpx
        .title-desc-rating
            color #e26353
    
    .video-content-desc
        color #736e6d
        font-size 25rpx
        otw()
        padding-left 10rpx
        padding-right 15rpx
        hlh(50rpx)
        
    .play-vip
        otw()
        height 210rpx
        position relative
        margin-top 10rpx
        &::before
            content ''
            wh(750rpx, 1rpx)
            background-color #bfbfbf
            position absolute
            left 0rpx
            z-index 200
            top 10rpx
            transform scaleY(0.4)
        &::after
            content ''
            wh(750rpx, 1rpx)
            background-color #bfbfbf
            position absolute
            left 0rpx
            z-index 200
            top 208rpx
            transform scaleY(0.4)
        
        .vip-choose
            height 100rpx
    .comment
        height 500rpx
        width 100%
        top 470rpx
        z-index 200
    .plays-choose
        height calc(100%-200rpx)
        width 100%
        position fixed
        top 400rpx
        z-index 200
        
    .play-around
        .around-title
            height 100rpx
        .around-video
            height 210rpx
    .plays-holiday
        position relative
        &::before
            content ''
            wh(750rpx, 1rpx)
            background-color #bfbfbf
            position absolute
            z-index 200
            top -6rpx
            transform scaleY(0.4)
        .around-title
            height 100rpx
        .around-video
            height 220rpx
        
    
    
</style>
