<template>
    <div id="app">
        <!-- 加载动画 ( 状态传递给动画组件 ) -->
        <CssLoading v-if="researchInfo.judgeShow" />
        <!-- 数据成功时 去除加载动画 -->
        <div id="service_research_router" v-else>
            <!-- 内容渲染 -->
            <div class="research--box">
                <!-- <img :src="researchInfo.appPic" /> -->
                <h3> {{ researchInfo.name }} </h3>
                <p> {{ researchInfo.address }} </p>
            </div>
            <!-- 地点介绍 -->
            <div class="research--box">
                <Title :title_info = introduced_title />
                <p> {{ researchInfo.description }} </p>
            </div>
            <!-- 考察流程 -->
            <div class="research--box">
                <Title :title_info = flow_title />
                <ul class="flow--list">
                    <li v-for = "item in researchInfo.IPList">
                        <p> <b> {{ item.descriptionIP }} </b> </p>
                        <p> {{ item.timeLenght }} 分钟 </p>
                    </li>
                </ul>
            </div>
        </div>
    </div>
</template>

<script>
import      { mapActions, mapGetters } from 'vuex'
import      Title           from    './components/Title.vue'
import      CssLoading      from    './components/CssLoading.vue'
const       components = { Title, CssLoading }

export default {
    name: 'app',
    data() {
        return {
            introduced_title: {
                title: '地点介绍',
                backgroundColor: '#FFF'
            },
            flow_title: {
                title: '考察流程',
                backgroundColor: '#FFF'
            }
        }
    }
    ,methods: {
        ...mapActions([ 'setResearchInfo' ])               // 目的: axios 获取家具信息
    }
    ,created() {
        this.setResearchInfo()
    }
    ,watch: {
        researchInfo: function(val){}
    }
    ,computed: {
        ...mapGetters({ researchInfo: 'researchInfo' })
    }
    ,components: components
}
</script>

<style lang="sass" scoped>
@import './sass/main'

#app
    +bC($base-color)
    +pB(1px)

#service_research_router
    +bC($base-color)
    /* 设置MuseUI - App Bar样式 */
    +auto--AppBar
    // 统一box样式
    .research--box
        +auto--contentBox($autoMargin,rgba($title-color,.7),$title-size,rgba($title-color,.7),$text-size)
        +bC($F)
        >img
            @extend %imgCover
            +REM(margin-bottom,$autoMargin)
    // 考察流程
    .flow--list li
        +REM(padding-top,$autoMargin/2)
        +REM(padding-bottom,$autoMargin/2)
        overflow: hidden
        @extend %auto--borderBottomSoild                    // 通用修饰下划线
        >p
            @extend %dib
            @extend %justify
            &:first-child
                width: 80%
                padding-right: 5%
                float: left
            &:last-child
                width: 15%
                float: right
            >b
                font-weight: normal
                color: $title-color

// 加载动画
.peeek-loading
  background-color: rgb(255,94,27)
  overflow: hidden
  position: absolute
  top: 0
  bottom: 0
  left: 0
  right: 0
  height: 100%
  width: 100%
  ul
    position: absolute
    left: calc(50% - 0.7em)
    top: calc(50% - 4.2em)
    display: inline-block
    text-indent: 2.8em
    li:after, &:after
      width: 1.4em
      height: 1.4em
      background-color: #fff
      border-radius: 100%
    li:after
      content: ""
      display: block
    &:after
      content: ""
      display: block
      position: absolute
      top: 2.8em
  li
    position: absolute
    padding-bottom: 5.6em
    top: 0
    left: 0
    &:nth-child(1)
      transform: rotate(0deg)
      animation-delay: 0.125s
      &:after
        animation-delay: 0.125s
    &:nth-child(2)
      transform: rotate(36deg)
      animation-delay: 0.25s
      &:after
        animation-delay: 0.25s
    &:nth-child(3)
      transform: rotate(72deg)
      animation-delay: 0.375s
      &:after
        animation-delay: 0.375s
    &:nth-child(4)
      transform: rotate(108deg)
      animation-delay: 0.5s
      &:after
        animation-delay: 0.5s
    &:nth-child(5)
      transform: rotate(144deg)
      animation-delay: 0.625s
      &:after
        animation-delay: 0.625s
    &:nth-child(6)
      transform: rotate(180deg)
      animation-delay: 0.75s
      &:after
        animation-delay: 0.75s
    &:nth-child(7)
      transform: rotate(216deg)
      animation-delay: 0.875s
      &:after
        animation-delay: 0.875s
    &:nth-child(8)
      transform: rotate(252deg)
      animation-delay: 1s
      &:after
        animation-delay: 1s
    &:nth-child(9)
      transform: rotate(288deg)
      animation-delay: 1.125s
      &:after
        animation-delay: 1.125s
    &:nth-child(10)
      transform: rotate(324deg)
      animation-delay: 1.25s
      &:after
        animation-delay: 1.25s
    animation: dotAnimation 2.5s infinite
    &:after
      animation: dotAnimationTwo 2.5s infinite

@keyframes dotAnimation
  0%, 55%, 100%
    padding: 0 0 5.6em 0

  5%,50%
    padding: 2.8em 0


@-webkit-keyframes dotAnimationTwo
  0%, 55%, 100%
    opacity: 1
    transform: scale(1)

  5%,50%
    opacity: .5
    transform: scale(0.5)

.mu-checkbox-icon-uncheck, .mu-checkbox-icon-checked
    color: $theme-color !important
</style>

