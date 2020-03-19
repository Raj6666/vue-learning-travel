<template>
    <div class="icons">
        <swiper :options="swiperOption">
            <swiper-slide v-for="(page, pageIndex) of pages" :key="pageIndex">
                <div class="icon" v-for="item of page" :key="item.id">
                    <div class="icon-img">
                        <img class="icon-img-content" :src="item.imgUrl"/>
                    </div>
                    <p class="icon-desc">{{item.desc}}</p>
                </div>
            </swiper-slide>
        </swiper>
    </div>
</template>

<script>
export default {
    name: 'HomeIcons',
    props: {
        list: Array
    },
    data: () => ({
        swiperOption: {
            autoplay: false
        }
    }),
    computed: {
        pages() {
            const pages = [];
            this.list.forEach((item, index) => {
                const pageIndex = Math.floor(index / 8);
                if (!pages[pageIndex]) {
                    pages[pageIndex] = [];
                }
                pages[pageIndex].push(item);
            });
            return pages;
        }
    }
};
</script>

<style lang="stylus" scoped>
    @import '~styles/variables.styl'
    @import '~styles/mixins.styl'
    .icons >>> .swiper-container
        height: 0
        padding-bottom :50%
    .icons
        margin-top: .1rem
        .icon
            position: relative
            overflow: hidden
            float: left
            width: 25%
            height: 0
            padding-bottom: 25%
            .icon-img
                position: absolute
                top: 0
                left: 0
                right: 0
                bottom: .44rem
                box-sizing: border-box
                padding: .1rem
                .icon-img-content
                    display: block
                    margin: 0 auto
                    height: 100%
            .icon-desc
                position: absolute
                left: 0
                right: 0
                bottom: 0
                text-align: center
                line-height: .44rem
                height: .44rem
                color: $darkTextColor
                ellipsis()
</style>
