<template>
    <div>
        <HomeHeader></HomeHeader>
        <HomeSwiper :list="swiperList"></HomeSwiper>
        <HomeIcons :list="iconList"></HomeIcons>
        <HomeRecommend :list="recommendList"></HomeRecommend>
        <HomeWeekend :list="weekendList"></HomeWeekend>
    </div>
</template>

<script>
import { mapState } from 'vuex';
import axios from 'axios';
import HomeHeader from './components/Header';
import HomeSwiper from './components/Swiper';
import HomeIcons from './components/Icons';
import HomeRecommend from './components/Recommend';
import HomeWeekend from './components/Weekend';

export default {
    name: 'Home',
    data: () => ({
        lastCity: '',
        swiperList: [],
        iconList: [],
        recommendList: [],
        weekendList: []
    }),
    computed: {
        ...mapState(['city'])
    },
    components: {
        HomeHeader,
        HomeSwiper,
        HomeIcons,
        HomeRecommend,
        HomeWeekend
    },
    methods: {
        getHomeInfo() {
            axios.get('/api/index.json?city=' + this.city)
                .then(this.getHomeInfoSucc);
        },
        getHomeInfoSucc(res) {
            const result = res.data;
            if (result.ret) {
                const data = result.data;
                this.swiperList = data.swiperList;
                this.iconList = data.iconList;
                this.recommendList = data.recommendList;
                this.weekendList = data.weekendList;
            }
        }
    },
    mounted() {
        this.getHomeInfo();
        this.lastCity = this.city;
    },
    activated() {
        if (this.lastCity !== this.city) {
            this.getHomeInfo();
            this.lastCity = this.city;
        }
    }
};
</script>

<style>

</style>
