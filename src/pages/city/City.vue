<template>
  <div>
      <CityHeader></CityHeader>
      <CitySearch
        :cities="cities"
      ></CitySearch>
      <CityList
        :hotCities="hotCities"
        :cities="cities"
        :letter="letter"
      ></CityList>
      <CityAlphabet
        :cities="cities"
        @change="handleLetterChange"
      ></CityAlphabet>
  </div>
</template>

<script>
import axios from 'axios';
import CityHeader from './components/Header';
import CitySearch from './components/Search';
import CityList from './components/List';
import CityAlphabet from './components/Alphabet';

export default {
    name: 'City',
    components: {
        CityHeader,
        CitySearch,
        CityList,
        CityAlphabet
    },
    data: () => ({
        hotCities: [],
        cities: {},
        letter: ''
    }),
    methods: {
        getCityInfo() {
            axios.get('/api/city.json')
                .then(this.handleGetCityInfoSucc);
        },
        handleGetCityInfoSucc(res) {
            const resultData = res.data;
            if (resultData.ret) {
                this.hotCities = resultData.data.hotCities;
                this.cities = resultData.data.cities;
            }
        },
        handleLetterChange(letter) {
            this.letter = letter;
        },
        preventDocumentTouchDefault(event) {
             event.preventDefault();
        }
    },
    mounted() {
        this.getCityInfo();
    },
    activated() {
          document.addEventListener('touchmove', this.preventDocumentTouchDefault, {
                passive: false
          });
    },
    deactivated() {
        document.removeEventListener('touchmove', this.preventDocumentTouchDefault);
    }
};
</script>

<style lang="stylus" scoped>

</style>
