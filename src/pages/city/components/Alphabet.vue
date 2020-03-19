<template>
    <ul class="list">
        <li
            class="item"
            v-for="item of letters"
            :key="item"
            @click="changeCurrentLetter"
            @touchstart.prevent="handleTouchStart"
            @touchmove="handleTouchMove"
            @touchend="handleTouchEnd"
            :ref="item"
        >{{item}}</li>
    </ul>
</template>

<script>
export default {
    name: 'CityAlphabet',
    props: {
        cities: Object
    },
    computed: {
        letters() {
            const letters = Object.keys(this.cities);
            return letters;
        }
    },
    data: () => ({
        touchStatus: false,
        startY: 0,
        timer: null
    }),
    updated() {
        this.startY = this.$refs.A[0].offsetTop;
    },
    methods: {
        changeCurrentLetter(e) {
            this.$emit('change', e.target.innerText);
        },
        handleTouchStart() {
            this.touchStatus = true;
        },
        handleTouchMove(e) {
            if (this.touchStatus) {
                if (this.timer) {
                    clearTimeout(this.timer);
                }
                this.timer = setTimeout(() => {
                    const startY = this.startY;
                    const touchY = e.touches[0].clientY - 79;
                    const index = Math.floor((touchY - startY) / 22);

                    if (index >= 0 && index < this.letters.length) {
                        this.$emit('change', this.letters[index]);
                    }
                }, 16);
            }
        },
        handleTouchEnd() {
            this.touchStatus = false;
        }
    }
};
</script>

<style lang="stylus" scoped>
     @import '~styles/variables.styl'
     .list
        display: flex
        flex-direction: column
        justify-content: center
        position: absolute
        top: 1.58rem
        right: 0
        bottom: 0
        width: .4rem
        .item
            line-height: .44rem
            text-align: center
            color: $bgColor
</style>
