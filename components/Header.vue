<template>
  <header class="theme-header p--y-03">
    <div class="container">
      <a href="/" class="header-title-container font--decoration-none">
        <div class="header-logo-container">
          <img src="../assets/imgs/logo.png" alt="РСХБ лого" class="w--10 h--10 m--r-03">
        </div>
        <div class="font--decoration-none">
          <p class="color--gray-text font--bolder font--20-28">Библиотека элементов</p>
          <p class="color--gray-label font--12-14">от Россельхозбанка</p>
      </div>
    </a>
      <button class="theme-button--medium theme-button--main theme-button">Все элементы</button>
    </div>
  </header>
</template>

<style lang="scss" scoped>

header {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  z-index: 10;
}

  img {
  max-width: 100%;
  }
  .theme-header {
    box-shadow: 0 4px 25px rgb(0 0 0 / 7%);
    background: #ffffff
  }
  .container {
    display: flex;
    justify-content: space-between;
    margin: 0 auto;

    @media only screen and (min-width: 720px) {
        padding-left: 40px;
        padding-right: 40px;
    }
    @media only screen and (min-width: 1440px) {
      min-width: 1440px;
      padding-left: 80px;
      padding-right: 80px;
    }

  }
  .header-title-container {
    display: flex;
  }
</style>
