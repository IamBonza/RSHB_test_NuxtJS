<template>
  <div class="mobile--12 tablet--8 desktop--9 ">

    <template v-if="resumesList.length">
      <div>
        <div class="theme-card--resume border--gray-stroke-1 m--b-05 resume-card-container" v-for="resume in resumesList">
          <div class="grid--row">
            <div class="mobile--12 tablet--2 desktop--1">
              <div class="theme-card--resume-photo">
                <img src="http://uikit.test.devrshb.tech/img/rshb-vue-kit-user-avatar.40b1ee27.svg" alt="photo">
              </div>
            </div>
            <div class="grid--flex grid--flex-column mobile--12 tablet--10 desktop--8">
              <h4 class="theme-card--resume-position m--b-01 color--gray-text font--bolder font--24-32">{{ resume.desired_position }}</h4>
              <h4 class="m--b-04 color--general-default font--bolder font--24-32">{{ resume.salary }} &#8381</h4>
              <div class="grid--flex grid--flex-column m--b-02">
                <span class="font--12-20 color--gray-label font--normal">Опыт работы</span>
                <span class="color--gray-text font--normal font--14-20" v-if="resume.totalJobTime !== 'Без опыта работы'">
                  {{ resume.totalJobTime }}</span>
                <span class="color--gray-text font--normal font--14-20" v-if="resume.totalJobTime === 'Без опыта работы'">Без опыта
                  работы</span>
              </div>
              <div class="grid--flex grid--flex-column m--b-05">
                <span class="color--gray-text font--normal font--12-20" v-if="resume.totalJobTime !== 'Без опыта работы'">Последнее
                  место работы</span>
                <span  v-if="resume.totalJobTime !== 'Без опыта работы'">
                  <span class="color--gray-text font--bolder font--14-20">{{ resume.experience }},</span>
                  <span class="color--gray-text font--normal font--14-20"> {{ resume.jobPeriod }}</span>
                </span>
              </div>
            </div>
            <div class="mobile--12 tablet--10 desktop--3 grid--flex grid--justify-end updated-time">
              <span class="font--12-20 font--normal color--gray-label">Обновлено {{ resume.updateDate }}</span>
            </div>
          </div>
          <div class="grid--row">
            <div class="mobile--12 tablet--2 desktop--1"></div>
            <div class="grid--flex-center mobile--12 tablet--10 desktop--3 tablet--offset-2">
              <div class="theme-card--resume-icon"></div>
              <div class="grid--flex grid--flex-column">
                <span class="color--gray-label font--normal font--12-16">{{ resume.region }}</span>
                <span class="color--gray-text font--bolder font--14-20">{{ resume.residence }}</span>
              </div>
            </div>
            <div class="grid--flex-center mobile--12 tablet--10 desktop--4 tablet--offset-2" v-if="resume.with_lodging">
              <div class="theme-card--resume-icon"></div>
              <span class="color--gray-text font--bolder font--14-20">Ищу работу с проживанием</span>
            </div>
            <div class="grid--flex-center mobile--12 tablet--10 desktop--4 tablet--offset-2" v-if="resume.family === 'С семьей' || resume.family === 'С командой'">
              <div class="theme-card--resume-icon"></div>
              <span class="color--gray-text font--bolder font--14-20" v-if="resume.family === 'С семьей'">
                Ищу работу с семьей/ {{ resume.familyCount }}
              </span>
              <span class="color--gray-text font--bolder font--14-20" v-if="resume.family === 'С командой'">
                Ищу работу с командой/ {{ resume.familyCount }}
              </span>
            </div>
          </div>
          <div class="grid--row">

          </div>
        </div>
        <button class="m--b-02 theme-button w--full theme-button--large theme-button--green-stroke"
          @click="loadMore"
        >Загрузить еще</button>
        <Pagination :pagination="pagination"/>
      </div>
    </template>
    <div v-else>Резюме не найдено</div>
  </div>
</template>

<script>
  export default {
    name: 'ResumesList',
    props: ['resumesList', 'pagination'],
    methods: {
      loadMore() {
        this.$emit('loadMore')
      }
    }
  }

</script>

<style lang="scss" scoped>
.resume-card-container {
  position: relative;
  @media screen and (max-width: 1440px) {
    padding-bottom: 46px;
  }

}
.updated-time {
  @media screen and (max-width: 1440px) {
    position: absolute;
    bottom: 16px;
    right: 16px;
  }

}

</style>
