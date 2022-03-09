<template>
  <div>
    <Header/>
    <Nuxt/>
  </div>
</template>


<style lang="scss">
  *{
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: Ubuntu, sans-serif;
  }
  .w--full {
    width: 100%;
  }

  .grid--flex {
    display: flex;
  }

  .grid--flex-column {
    flex-direction: column;
  }

  button {
    border: 0;
  }
  .p--y-03 {
    padding-top: 12px;
    padding-bottom: 12px;
  }
  .m--t-03 {
    margin-top: 12px;
  }
  .m--r-03 {
    margin-right: 12px;
  }
  .m--b-05 {
    margin-bottom: 20px;
  }
  .m--b-07 {
    margin-bottom: 28px;
  }
  .w--10 {
    width: 40px;
  }
  .h--6 {
    height: 24px;
  }
  .h--10 {
    height: 40px;
  }
  .font--12-14 {
    font-size: 12px;
    line-height: 14px;
    letter-spacing: 0.1px;
  }
  .font--18-28 {
    font-size: 18px;
    line-height: 28px;
    letter-spacing: 0.3px;
  }
  .font--20-28 {
    font-size: 20px;
    line-height: 28px;
  }
  .font--32-40 {
    font-size: 32px;
    line-height: 40px;
  }
  .font--normal {
    font-weight: 400;
  }
  .font--bolder {
    font-weight: 500;
  }
  .font--decoration-none {
    text-decoration: none;
  }
  .color--gray-text {
    color: #1c1c1c
  }

  .color--gray-label {
    color: #7d7e7f;
  }
  .theme-button {
    display: flex;
    justify-content: center;
    align-items: center;
    cursor: pointer;
    text-decoration: none;
    font-weight: 500;
  }
  .theme-button--main {
    color: #ffffff;
    background: #42ab44;
  }
  .theme-button--medium {
    padding-left: 36px;
    padding-right: 36px;
    height: 40px;
    border-radius: 20px;
    letter-spacing: 0.1px;
    font-size: 14px;
    line-height: 20px;
  }
  .theme-control--input {
    height: 48px;
    border: 1px solid #c9cacc;
    border-radius: 12px;
    background: #ffffff
  }

  .border--gray-stroke-1 {
    border: 1px solid #c9cacc;
  }

  .mobile--12 {
    @media only screen and (max-width: 719px){
      flex-basis: calc(12 * 8.33333% - 16px);
      max-width: calc(12 * 8.33333% - 16px);
      margin-left: 8px;
      margin-right: 8px;
    }
  }

  .tablet--4 {
    @media only screen and (min-width: 720px) and (max-width: 1439px) {
      flex-basis: calc(4 * 8.33333% - 20px);
      max-width: calc(4 * 8.33333% - 20px);
      margin-left: 10px;
      margin-right: 10px;
    }
  }
  .tablet--8 {
    @media only screen and (min-width: 720px) and (max-width: 1439px){
      flex-basis: calc(8 * 8.33333% - 20px);
      max-width: calc(8 * 8.33333% - 20px);
      margin-left: 10px;
      margin-right: 10px;
    }
  }

  .desktop--3 {
    @media only screen and (min-width: 1440px) {
      flex-basis: calc(3 * 8.33333% - 20px);
      max-width: calc(3 * 8.33333% - 20px);
      margin-left: 10px;
      margin-right: 10px;
    }
  }
  .desktop--9 {
      @media only screen and (min-width: 1440px){
      flex-basis: calc(9 * 8.33333% - 20px);
      max-width: calc(9 * 8.33333% - 20px);
      margin-left: 10px;
      margin-right: 10px;
    }
  }





</style>
