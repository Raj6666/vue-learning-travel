<template>
    <div>
        <div class="search">
            <input class="search-input" v-model="keyWord"  type="text" placeholder="输入城市名或拼音">
        </div>
        <div class="search-content" v-show="keyWord" ref="searchContent">
            <ul>
                <li class="search-item border-bottom"
                    v-for="item of list"
                    :key="item.id"
                     @click="handleCityClick(item.name)">
                {{item.name}}</li>
                <li class="search-item border-bottom" v-show="hasNoContent">没有找到匹配项</li>
            </ul>
        </div>
    </div>
</template>

<script>
import Bscroll from 'better-scroll';
import { mapMutations } from 'vuex';

export default {
    name: 'CitySearch',
    props: {
        cities: Object
    },
    data: () => ({
        keyWord: '',
        list: [],
        timer: null
    }),
    computed: {
        hasNoContent() {
            return this.list.length <= 0;
        }
    },
     methods: {
        handleCityClick(city) {
            this.changeCity(city);
            this.$router.push('/');
        },
        ...mapMutations(['changeCity'])
    },
    watch: {
        keyWord() {
            if (this.timer) {
                clearTimeout(this.timer);
            }
            if (!this.keyWord) {
                this.list = [];
                return;
            }
            this.timer = setTimeout(() => {
                const searchedList = [];
                Object.keys(this.cities).forEach((key) => {
                    this.cities[key].forEach((city) => {
                            if (
                                city.spell.indexOf(this.keyWord) > -1 ||
                                city.name.indexOf(this.keyWord) > -1) {
                                searchedList.push(city);
                            }
                        });
                });
                this.list = searchedList;
            }, 100);
        }
    },
    mounted() {
        this.scroll = new Bscroll(this.$refs.searchContent);
    },
};
</script>

<style lang="stylus" scoped>
    @import '~styles/variables.styl'
    .search
        height: .72rem
        background: $bgColor
        padding: 0 .1rem
        .search-input
            box-sizing: border-box
            width: 100%
            height: .62rem
            line-height: .62rem
            padding: 0 .1rem
            text-align: center
            border-radius: .06rem
            color: #666
            touch-action: none
    .search-content
        z-index: 1
        overflow: hidden
        position: absolute
        top: 1.58rem
        left: 0
        right: 0
        bottom: 0
        background: #fff
        .search-item
            line-height: .62rem
            padding-left: .2rem
            color: #666
</style>
