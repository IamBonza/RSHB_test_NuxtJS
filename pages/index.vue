<template>
  <main class="main-container">
    <div class="m--b-07 main-title">
      <h1 class="font--32-40 font--bolder">
        Уникальная база резюме
        <p class="m--t-03 color--gray-label font--normal font--18-28">в сфере АПК</p>
      </h1>
    </div>

    <div class="content-container">
      <div class="filters-container mobile--12 tablet--4 desktop-3">
        <BaseMultiSelect label="Регион"
                         :autocomplete="true"
                         :value.sync="selectedRegions"
                         :items="regionList"
                         :search-value.sync="searchedRegionValue"
        />
        <BaseMultiSelect label="Город"
                         :autocomplete="true"
                         :value.sync="selectedCities"
                         :items="citiesList"
                         :search-value.sync="searchedCitiesValue"
                         :disabled="!selectedRegions.length"
        />
      </div>
        <ResumesList :resumesList="this.resumes" :pagination="paginationInfo" @loadMore="loadMore"/>

    </div>
  </main>
</template>

<script>
import ResumesList from '../components/ResumesList';
import axios from 'axios';
import Pagination from '../components/Pagination';
import BaseMultiSelect from '../components/UI/BaseMultiSelect';
export default {
  name: 'IndexPage',
  components: {Pagination, ResumesList, BaseMultiSelect},
  data() {
    return {
      regionList: [],
      citiesList: [],
      selectedRegions: [],
      selectedCities: [],
      searchedRegionValue: '',
      searchedCitiesValue: '',
      resumes: [],
      isLoadMore: false,
      paginationInfo: {
        totalResumes: 0,
        totalPages: 0,
        perPage: 25,
        resumesLength: 0
      }
    }
  },
  async fetch() {
      const currentPage = this.currentPage
      await this.getRegions()
      await this.getResumes(currentPage)
  },


  methods: {
    async getRegions() {
      let regionList = await axios.post('https://svoefermerstvo.test.devrshb.tech/api/ext/dictionaries/get-dictionary-data', {dictionary: "region"})
      this.regionList = regionList.data.result
    },

    async getCitiesByRegions(regions) {
      let regionIdList = regions.map(reg => {
        return reg.id
      })
      let citiesList = await axios.post('https://svoefermerstvo.test.devrshb.tech/api/ext/dictionaries/get-dictionary-data', {"dictionary":"settlement","search":"","filter":{"dict_item_parent_id.keyword":regionIdList}})
      this.citiesList = citiesList.data.result
    },

    async getResumes(currentPage) {
      const extend = this.$route.query.extend
      const size = 25
      const from = currentPage * size
      let link = `https://svoefermerstvo.test.devrshb.tech/api/hiring/resume?from=${from}&size=${size}&sort%5Bid%5D=desc&filters%5Bsalary%5D%5Bgte%5D=0&filters%5Bsalary%5D%5Blte%5D=999999`
      if (this.selectedRegions.length) {
        let regions = [];
        this.selectedRegions.forEach((reg, index) => {
          regions.push(`&filters[region_id.keyword][${index}]=${reg.id}`)
        })
        link += regions.join('')
      }
      if (this.selectedCities.length) {
        let cities = [];
        this.selectedCities.forEach((city, index) => {
          cities.push(`&filters[residence_id.keyword][${index}]=${city.id}`)
        })
        link += cities.join('')
      }
      const data = axios.get(link)
      const result = await data;
      const rawResumes = result.data.result.resumes;

      this.$set(this.paginationInfo, 'totalResumes', result.data.result.total)
      this.$set(this.paginationInfo, 'totalPages', Math.ceil(result.data.result.total / this.paginationInfo.perPage))

      if (!extend) {
        this.resumes.splice(0)
      }
      rawResumes.forEach((resume) => {
        this.resumes.push(this.prepareResume(resume))
      })
      this.$set(this.paginationInfo, 'resumesLength', this.resumes.length)

    },

    prepareResume(obj) {
      let result = {}
      // подготовка желаемой должности
      let tempArray = [];
      obj.desired_position.forEach((elem) => {
        tempArray.push(elem.desired_position)
      })
      result.desired_position = tempArray.join(', ')

      // подготовка зарплаты
      const formatter = new Intl.NumberFormat('ru');
      result.salary = formatter.format(obj.salary)

      //подготовка места работы
      result.experience = obj.experience[0] ? obj.experience[0].job_facility : 'Без опыта работы'

      //подготовка дат работы
      let tempJobStart = obj.experience[0] ? new Date(obj.experience[0].start_date) : '';
      tempJobStart = tempJobStart ? tempJobStart.toLocaleDateString('ru-RU', {month: 'long', year: 'numeric'}) : '';
      tempJobStart = tempJobStart.slice(0, -3);

      let tempJobEnd;
      if (obj.experience[0] && obj.experience[0].end_date) {
        tempJobEnd = new Date(obj.experience[0].end_date)
      } else {
        tempJobEnd = ''
      }
      tempJobEnd = tempJobEnd ? tempJobEnd.toLocaleDateString('ru-RU', {month: 'long', year: 'numeric'}) : '';
      tempJobEnd = tempJobEnd.slice(0, -3)

      let jobPeriod = '';

      if (tempJobStart && tempJobEnd) {
        jobPeriod = `${tempJobStart} - ${tempJobEnd}`
      } else if (tempJobStart && tempJobEnd === '') {
        jobPeriod = `${tempJobStart} - по настоящее время`
      }
      result.jobPeriod = jobPeriod


      // Рассчет стажа работы

      let totalJobTime, dateStart, dateEnd, yearStart, yearEnd, monthEnd, nowDate, totalYears

      dateStart = obj.experience[0] ? new Date(obj.experience[0].start_date) : '';
      dateEnd = obj.experience[0] && obj.experience[0].end_date ? new Date(obj.experience[0].end_date) : '';
      yearStart = dateStart ? dateStart.getFullYear() : '';
      if (dateStart && dateEnd) {
        yearEnd = dateEnd.getFullYear();
        monthEnd = dateEnd.getMonth() + 1;
      } else if (dateStart && dateEnd === '') {
        nowDate = new Date();
        yearEnd = nowDate.getFullYear();
        monthEnd = nowDate.getMonth() + 1;
      } else {
        yearEnd = '';
        monthEnd = '';
      }

      totalYears = yearEnd - yearStart

      if (totalYears === 1) {
        totalJobTime = totalYears + ' год';
      } else if (totalYears >= 2 && totalYears <= 4) {
        totalJobTime = totalYears + ' года';
      } else if (totalYears >= 5 && totalYears <= 20) {
        totalJobTime = totalYears + ' лет';
      }

      if (monthEnd === 1) {
        totalJobTime += ' и 1 месяц'
      } else if (monthEnd >= 2 && monthEnd <= 4) {
        totalJobTime += ` и ${monthEnd} месяца`
      } else if (monthEnd >= 5 && monthEnd <= 20) {
        totalJobTime += ` и ${monthEnd} месяцев`
      }

      if (!obj.experience[0]) {
        totalJobTime = 'Без опыта работы'
      }
      result.totalJobTime = totalJobTime

      // подготовка места жительства

      result.residence = obj.residence;
      result.region = obj.region;

      // С проживанием или нет
      result.with_lodging = obj.with_lodging

      // с семьей или нет
      result.family = obj.family.family
      result.familyCount = obj.family.children_count + obj.family.adult_count
      result.family = obj.family.family
      result.familyCount = obj.family.children_count + obj.family.adult_count
      if (result.familyCount % 10 >= 0 && result.familyCount % 10 <= 1) {
        result.familyCount += ' человек'
      } else if (result.familyCount % 10 >= 2 && result.familyCount % 10 <= 4) {
        result.familyCount += ' человека'
      } else if (result.familyCount % 10 >= 5 && result.familyCount % 10 <= 9) {
        result.familyCount += ' человек'
      }

      // дата обновления
      let updateDate = new Date(obj.update_date);
      updateDate = updateDate.toLocaleDateString('ru-RU', {
        day: 'numeric',
        month: 'long',
        hour: 'numeric',
        minute: 'numeric'
      });
      result.updateDate = updateDate;
      result.id = obj.id
      return result
    },
    loadMore() {
      const page = this.currentPage + 1
      this.$router.push({query:{
        page: page + 1,
          extend: true,
        }})


    },

  },
  computed: {
    currentPage() {
      return this.$route.query.page - 1 || 0
    }
  },
  watch: {
    currentPage: {
      immediate: false,
      handler: function (newPage) {
        this.getResumes(newPage)
      }
    },
    selectedRegions: {
      handler: function (selectedRegions) {
        this.getCitiesByRegions(selectedRegions)
        this.getResumes(0)
      }
    },
    selectedCities (selectedCities) {
      this.getResumes(0)
    }

  }
}
</script>

<style lang="scss" scoped>
  .main-container {
    .main-title {
      margin-left: 10px;
      margin-right: 10px;
    }
    margin: 70px auto 0 auto;
    @media only screen and (min-width: 720px){
      padding-left: 40px;
      padding-right: 40px;
    }
    @media only screen and (min-width: 1440px) {
      max-width: 1440px;
      padding-left: 80px;
      padding-right: 80px;
      }
      .content-container {
        display: flex;
        .filters-container {
          width: 100%;
          height: 100vh;
            @media only screen and (max-width: 719px) {
            display: none;
            }
            @media only screen and (min-width: 1440px) {
              flex-basis: calc(3 * 8.33333% - 20px);
              max-width: calc(3 * 8.33333% - 20px);
              margin-left: 10px;
              margin-right: 10px;
            }

          }
      }

    }
</style>
