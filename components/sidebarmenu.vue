<template>
    <div class="sidebar">
      <div class="logo-section">
        <div class="logo"><img src="/assets/1.png" alt="logo"></div>
        <h1 class="company-name">Medical School</h1>
      </div>
      <div class="profile-section">
        <h2 class="section-title">Профиль</h2>
        <div class="profile-content">
          <div class="avatar">
            <img src="/assets/2.png" alt="Аватар">
          </div>
          <div class="profile-info">
            <div class="user-name">Иван Иванов</div>
            <div class="user-level">Ур. 3</div>
          </div>
        </div>
        <button class="edit-btn">Редактировать</button>
      </div>
      <div class="info-section">
        <h2 class="section-title">Информация</h2>
        <ul class="menu-links">
          <li><nuxt-link to="/">О сайте</nuxt-link></li>
          <li><nuxt-link to="/">Правила</nuxt-link></li>
          <li><nuxt-link to="/forum/">Форум</nuxt-link></li>
        </ul>
      </div>
      <div class="tasks-section">
        <h2 class="section-title">Задачник</h2>
        <ul class="menu-links">
          <li><nuxt-link to="/tasks/">Архив задач</nuxt-link></li>
          <li><nuxt-link to="/system-status/">Состояние системы</nuxt-link></li>
          <li><nuxt-link to="/">Рейтинг</nuxt-link></li>
          <li><nuxt-link to="/">Курсы</nuxt-link></li>
        </ul>
      </div>
      <div class="methods-section">
        <h2 class="section-title">Методичка</h2>
        <ul class="menu-links">
          <li><nuxt-link to="/">Клинические рекомендации</nuxt-link></li>
          <li><nuxt-link to="/">Атлас анатомии</nuxt-link></li>
          <li><nuxt-link to="/">Фармакология</nuxt-link></li>
        </ul>
      </div>
    </div>
  </template>
  <script>
  export default {
    name: 'SidebarMenu'
  }
  </script>
  <style scoped>
  @font-face {
    font-family: 'Montserrat';
    src: url('/assets/fonts/Montserrat-Regular.woff2') format(woff2);
    font-weight: 400;
    font-style: normal;
  }
  @font-face {
    font-family: 'Montserrat Medium';
    src: url('/assets/fonts/Montserrat-Medium.woff2') format(woff2);
    font-weight: 500;
    font-style: normal;
  }
  .sidebar {
    width: 300px;
    padding: 0 1%;
  }
  .logo-section {
    display: flex;
    align-items: center;
    padding: 1% 5%;
    margin-bottom: 5%;
    background: #4ba285;
    border-radius: 0.5em;
  }
  .logo {
    margin-right: 15%;
  }
  .logo img{
    width: 30px;
  }
  .company-name {
    font-size: 16px;
    color: #f5f5f5;
    font-weight: normal;
    font-family: 'Montserrat Medium';
  }
  .section-title {
    font-size: 15px;
    color: #f5f5f5;
    font-weight: normal;
    font-family: 'Montserrat Medium';
    margin: 5% 0 1%;
    padding-bottom: 5px;
    text-align: center;
  }
  .profile-section {
    background: #4ba285;
    border-radius: 0.5em;
    padding: 0.5% 5% 5% 5%;
    margin-bottom: 5%;
  }
  .profile-content {
    display: flex;
    align-items: center;
    gap: 0.5vw;
    margin: 1% 0;
  }
  .avatar img {
    width: 32px;
    object-fit: cover;
  }
  .profile-info {
    display: flex;
    flex-direction: row;
  }
  .user-name {
    font-family: 'Montserrat';
    font-size: 14px;
    margin-right: 100px;
    color: white;
  }
  .user-level {
    font-size: 14px;
    color: white;
    font-family: 'Montserrat';
  }
  .edit-btn {
    width: 100%;
    background: #6ebb91;
    border: none;
    color: white;
    border-radius: 4px;
    padding: 2.5%;
    font-size: 14px;
    cursor: pointer;
    transition: all 0.3s;
    font-family: 'Montserrat';
  }
  .edit-btn:hover {
    background-color: #f5f5f5;
    border-color: #4caf50;
    color: #4caf50;
  }
  .info-section{
    background: #4ba285;
    border-radius: 0.5em;
    padding: 0.5% 5%;
    margin-bottom: 5%;
    height: 30vh;
  }
  .menu-links {
    list-style: none;
    padding: 0;
  }
  .menu-links li {
    margin: 1% 0;
  }
  .menu-links a {
    color: white;
    text-decoration: none;
    font-size: 15px;
    font-family: 'Montserrat';
  }
  .menu-links a:hover {
    color: #333;
  }
  .tasks-section{
    background: #4ba285;
    border-radius: 0.5em;
    padding: 0.5% 5%;
    margin-bottom: 5%;
    height: 25vh;
  }
  .methods-section{
    background: #4ba285;
    border-radius: 0.5em;
    padding: 0.5% 5%;
    height: 18vh;
  }
  </style>