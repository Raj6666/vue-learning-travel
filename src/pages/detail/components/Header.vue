<template>
    <div>
       <router-link tag="div" to="/" class="header-abs"
            v-show="showAbs"
        >
           <div class="iconfont back-icon">&#xe624;</div>
       </router-link>
       <div class="header-fixed"
            v-show="!showAbs"
            :style="opacityStyle"
        >
            <router-link to="/">
                <div class="iconfont back-icon">&#xe624;</div>
            </router-link>
            景点详情
       </div>
    </div>
</template>

<script>

export default {
    name: 'DetailHeader',
    data: () => ({
        showAbs: true,
        opacityStyle: {
            opacity: 0
        }
    }),
    methods: {
        handleScroll() {
            const top = document.documentElement.scrollTop;
            if (top > 60) {
                this.showAbs = false;
                let opacity = top / 140;
                opacity = opacity > 1 ? 1 : opacity;
                this.opacityStyle.opacity = opacity;
            } else {
                this.showAbs = true;
            }
        }
    },
    mounted() {
        window.addEventListener('scroll', this.handleScroll);
    },
    destroyed() {
        window.removeEventListener('scroll', this.handleScroll);
    }
};
</script>

<style lang="stylus" scoped>
     @import '~styles/variables.styl'
    .header-abs
        position: absolute
        top: .2rem
        left: .2rem
        width: .8rem
        height: .8rem
        line-height: .8rem
        border-radius: .4rem
        background: rgba(0, 0, 0, .8)
        .back-icon
            text-align: center
            font-size: .4rem
            color: #fff
    .header-fixed
        z-index: 2
        position: fixed
        top: 0
        left: 0
        right: 0
        overflow: hidden
        height: $headerHeight
        line-height: $headerHeight
        text-align: center
        color: #fff
        background: $bgColor
        font-size: .32rem
        .back-icon
            position: absolute
            top: 0
            left: 0
            width: .64rem
            text-align: center
            font-size: .4rem
            color: #fff
</style>
