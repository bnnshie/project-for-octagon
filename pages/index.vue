<template>
  <div class="container">
    <SidebarMenu />
    <div class="main-content">
      <h1 class="page-title">О сайте Medical School</h1>
      <div class="description">
        <p>Medical School – это инновационная образовательная платформа для студентов медицинских вузов и практикующих врачей.</p>
        <p>Наш сайт предоставляет:</p>
        <ul>
          <li>Доступ к обширной базе учебных материалов;</li>
          <li>Интерактивные задачи и клинические кейсы;</li>
          <li>Систему рейтинга и мотивации;</li>
          <li>Форум для обсуждения сложных случаев;</li>
          <li>Актуальные клинические рекомендации.</li>
        </ul>
        <p>Мы помогаем медицинским специалистам непрерывно совершенствовать свои знания и навыки.</p>
      </div>
      <div class="image"><img src="/assets/3.png"></div>
    </div>
  </div>
</template>
<script>
import SidebarMenu from '~/components/sidebarmenu.vue'
export default {
  name: 'HomePage',
  components: {
    SidebarMenu
  }
}
</script>
<style scoped>
.container {
  display: flex;
  max-height: 100vh;
}
.main-content {
  flex: 1;
  padding: 0 1.5%;
}
.page-title {
  font-size: 1.5em;
  font-family: 'Montserrat Medium';
  font-weight: normal;
  padding-bottom: 1%;
  margin-bottom: 0;
  position: relative;
  display: inline-block; 
}
.page-title::after {
  content: '';
  position: absolute;
  bottom: 0;
  left: 0;
  width: 25%; 
  height: 2px;
  background-color: #4caf50;
}
.description {
  font-size: 1em;
  line-height: 1.6;
  font-family: 'Montserrat';
  margin-bottom: 3%;
}
.image {
  margin-top: 3%;
  text-align: center;
}
</style>