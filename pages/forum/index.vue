<template>
  <div class="container">
    <SidebarMenu />
    <div class="main-content">
      <h1 class="page-title">Форум</h1>
      <div class="topics-grid">
        <div 
          v-for="topic in topics" 
          :key="topic.id" 
          class="topic-card"
          :class="{ expanded: topic.expanded }">
          <div class="topic-header">
            <h3 class="topic-title">{{ topic.title }}</h3>
            <span class="topic-date">{{ formatDate(topic.date) }}</span>
          </div>
          <div class="topic-author">{{ topic.author }}</div>
          <div class="topic-content">
            <p>{{ topic.expanded ? topic.content : truncatedContent(topic.content) }}</p>
            <nuxt-link 
              v-if="topic.content.length > 80" 
              :to="`/forum/${topic.id}`" 
              class="expand-button">
              {{ topic.expanded ? 'Свернуть' : 'Подробнее...' }}
            </nuxt-link>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import SidebarMenu from '~/components/sidebarmenu.vue'

export default {
  name: 'ForumPage',
  components: {
    SidebarMenu
  },
  data() {
    return {
      topics: [
        {
          id: 1,
          title: 'Диагностика COVID-19',
          date: '2023-03-15',
          author: 'Смирнов А.',
          content: 'Обсуждение современных методов диагностики COVID-19. Какие тесты наиболее точны на ранних стадиях заболевания? Как интерпретировать результаты ПЦР-тестов с высокими значениями Ct? Поделитесь клиническим опытом дифференциальной диагностики с другими ОРВИ.',
          expanded: false
        },
        {
          id: 2,
          title: 'Реабилитация после инсульта',
          date: '2023-03-10',
          author: 'Козлова Е.',
          content: 'Какие современные методики нейрореабилитации вы используете в своей практике? Особый интерес представляют кейсы ранней мобилизации пациентов с ишемическим инсультом. Как вы оцениваете эффективность различных подходов к восстановлению когнитивных функций?',
          expanded: false
        },
        {
          id: 3,
          title: 'Педиатрическая фармакология',
          date: '2023-03-05',
          author: 'Васильев О.',
          content: 'Обсуждение особенностей дозирования лекарственных препаратов у детей разных возрастных групп. Какие формулы пересчета доз из взрослых в детские вы считаете наиболее корректными? Проблемы выбора лекарственных форм для детей раннего возраста.',
          expanded: false
        },
        {
          id: 4,
          title: 'Эндопротезирование суставов',
          date: '2023-02-28',
          author: 'Николаев Д.',
          content: 'Сравнительный анализ различных типов эндопротезов тазобедренного сустава. Какие критерии вы используете при выборе протеза? Особенности послеоперационного ведения пациентов с сопутствующим остеопорозом. Опыт применения индивидуальных протезов.',
          expanded: false
        },
        {
          id: 5,
          title: 'Аллергология в практике',
          date: '2023-02-20',
          author: 'Петрова М.',
          content: 'Современные подходы к диагностике и лечению поллинозов. Эффективность различных схем АСИТ. Как вы решаете проблему перекрестной аллергии? Интересны клинические случаи нестандартных аллергических реакций на лекарственные препараты.',
          expanded: false
        },
        {
          id: 6,
          title: 'Цифровая медицина',
          date: '2023-02-15',
          author: 'Иванова Т.',
          content: 'Обмен опытом использования телемедицинских технологий в повседневной практике. Какие платформы вы считаете наиболее удобными для дистанционных консультаций? Правовые аспекты телемедицины. Перспективы внедрения ИИ в диагностические процессы.',
          expanded: false
        }
      ]
    }
  },
  methods: {
    formatDate(dateString) {
      const options = { day: 'numeric', month: 'long' }
      return new Date(dateString).toLocaleDateString('ru-RU', options)
    },
    truncatedContent(content) {
      return content.length > 80 ? content.substring(0, 80) + '...' : content
    },
    toggleExpand(topic) {
      topic.expanded = !topic.expanded
    }
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
  margin: 0 auto;
  padding: 0 1.5%;
}
.page-title {
  margin-bottom: 2%;
  font-size: 1.5em;
  font-weight: normal;
  font-family: 'Montserrat Medium';
}
.topics-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 3%;
}
.topic-card {
  background: #f0f2f5;
  border-radius: 0.5em;
  padding: 3%;
  display: flex;
  flex-direction: column;
  min-height: 150px;
  margin-bottom: 3%;
  transition: all 0.3s ease;
  position: relative;
}
.topic-card:hover {
  transform: translateY(-3px);
  box-shadow: 0 3px 6px rgba(0,0,0,0.1);
}
.topic-header {
  display: flex;
  justify-content: space-between;
  margin-bottom: 2%;
}
.topic-title {
  font-size: 1em;
  font-family: 'Montserrat Medium';
  font-weight: normal;
  margin: 0;
  flex: 1;
}
.topic-date {
  font-size: 0.9em;
  font-family: 'Montserrat';
}
.topic-author {
  font-size: 0.9em;
  color: #777;
  font-family: 'Montserrat';
  margin-bottom: 2%;
}
.topic-content {
  font-size: 0.85em;
  line-height: 1.4;
  flex-grow: 1;
  overflow: hidden;
  font-family: 'Montserrat';
}
.topic-content p {
  margin: 0;
}
.expand-button {
  background: none;
  border: none;
  color: #4ba285;
  cursor: pointer;
  padding: 0;
  font-size: 0.95em;
  margin-top: 1%;
  text-align: left;
  font-family: 'Montserrat';
  text-decoration: none;
  display: inline-block;
}
.expand-button:hover {
  text-decoration: underline;
}
</style>