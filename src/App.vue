<template>
    <div id="app">
        <!-- 加载动画 -->
        <!--<div class="loader" v-if="researchInfo.judgeShow"></div>-->
        <!-- MuseUI 加载动画 -->

        <div class="mu-linear-progress" data-v-ecbf7674="" style="height: 5px; border-radius: 0;" v-if="researchInfo.judgeShow">
            <div class="mu-linear-progress-indeterminate" style="height: 5px; background-color: rgb(255,94,27); border-radius: 5px;"></div>
        </div>
        <!--<mu-linear-progress :size="5" color="rgb(255,94,27)" v-bind:style="{ width: '100%',height: '10px' }"v-if="researchInfo.judgeShow" />-->
        <!-- 数据成功时 去除加载动画 -->
        <div id="service_research_router" v-else>
            <!-- 内容渲染 -->
            <div class="research--box">
                <img :src="researchInfo.appPic" />
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
const       components = { Title }

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
        +auto--contentBox($autoMargin,rgba($title-color,.7),$text-size,rgba($title-color,.7),$text-size)
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
            &:first-child
                width: 85%
                float: left
            &:last-child
                width: 15%
                float: right
            >b
                font-weight: normal
                color: $title-color

// 加载动画
.loader
    &:before, &:after
        position: absolute
        top: 50%
        left: 50%
        border-radius: 50%
        border-style: solid
        border-top-color: #ECD078
        border-right-color: #C02942
        border-bottom-color: #542437
        border-left-color: #53777A
        content: ''
        transform: translate(-50%, -50%)
        animation: rotate 1.5s  infinite ease-in-out
    &:before
        border-width: 10vh
    &:after
        width: 30vh
        height: 30vh
        border-width: 2.5vh
        animation-direction: reverse

@keyframes rotate
    0%
        transform: translate(-50%, -50%) rotate(0)

    100%
        transform: translate(-50%, -50%) rotate(360deg)

.mu-checkbox-icon-uncheck, .mu-checkbox-icon-checked
    color: $theme-color !important
</style>

