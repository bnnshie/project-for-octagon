<template>
  <div class="container">
    <SidebarMenu />
    <div class="main-content">
      <h1 class="page-title">Архив задач</h1>
      <div class="title-underline"></div>
      <div class="page-description">
        <p>
          Архив задач содержит полный перечень учебных кейсов и практических заданий, 
          доступных в системе. Здесь вы можете:
        </p>
        <ul class="description-list">
          <li>Найти задачи по конкретным темам медицинской подготовки;</li>
          <li>Отслеживать свой прогресс (решенные/нерешенные задания);</li>
          <li>Ориентироваться на показатели сложности и решаемости;</li>
          <li>Быстро получать доступ к задачам с готовыми решениями.</li>
        </ul>
      </div>
      <div class="filters">
        <select v-model="filters.status" class="filter-select">
          <option value="all">Все задачи</option>
          <option value="solved">Решенные</option>
          <option value="unsolved">Не решенные</option>
        </select>
        <select v-model="filters.topic" class="filter-select">
          <option value="all">Все темы</option>
          <option v-for="topic in uniqueTopics" :key="topic" :value="topic">{{ topic }}</option>
        </select>
      </div>
      <div class="tasks-table">
        <div class="table-header">
          <div class="header-cell">Решено</div>
          <div class="header-cell">ID</div>
          <div class="header-cell">Название</div>
          <div class="header-cell">Тема</div>
          <div class="header-cell">Ответ</div>
          <div class="header-cell">Сложность</div>
          <div class="header-cell">Решаемость</div>
        </div>
         <div 
          v-for="task in filteredTasks" 
          :key="task.id" 
          class="table-row"
          @click="navigateToTask(task.id)">
          <div class="table-cell">{{ task.solved ? 'Да' : 'Нет' }}</div>
          <div class="table-cell">{{ task.id }}</div>
          <div class="table-cell">{{ task.title }}</div>
          <div class="table-cell">{{ task.topic }}</div>
          <div class="table-cell">{{ task.hasAnswer ? 'Есть' : 'Нет' }}</div>
          <div class="table-cell">{{ task.difficulty }}%</div>
          <div class="table-cell">{{ task.solvability }}%</div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import SidebarMenu from '~/components/sidebarmenu.vue'
export default {
  name: 'TasksArchive',
  components: {
    SidebarMenu
  },
  methods: {
    navigateToTask(taskId) {
      this.$router.push(`/tasks/${taskId}`)
    }
  },
  data() {
    return {
      tasks: [
        {
          id: 1,
          title: 'Анатомия сердца',
          topic: 'Анатомия',
          solved: true,
          hasAnswer: true,
          difficulty: 65,
          solvability: 78,
        },
        {
          id: 2,
          title: 'Фармакология антибиотиков',
          topic: 'Фармакология',
          solved: false,
          hasAnswer: true,
          difficulty: 80,
          solvability: 45,
        },
        {
          id: 3,
          title: 'Неврологический осмотр',
          topic: 'Неврология',
          solved: false,
          hasAnswer: false,
          difficulty: 70,
          solvability: 60,
        }
      ],
      filters: {
        status: 'all',
        topic: 'all'
      }
    }
  },
  computed: {
    uniqueTopics() {
      return [...new Set(this.tasks.map(task => task.topic))]
    },
    filteredTasks() {
      return this.tasks.filter(task => {
        if (this.filters.status === 'solved' && !task.solved) return false
        if (this.filters.status === 'unsolved' && task.solved) return false
        if (this.filters.topic !== 'all' && task.topic !== this.filters.topic) return false
        return true
      })
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
  margin-bottom: 1%;
  font-size: 1.5em;
  font-weight: normal;
  font-family: 'Montserrat Medium';
}
.title-underline {
  height: 2px;
  width: 5%;
  background-color: #4caf50;
  margin-bottom: 1.5%;
}
.page-description {
  line-height: 1.6;
  font-family: 'Montserrat';
}
.description-list {
  margin: 1.5% 0;
  padding-left: 2%;
}
.description-list li {
  position: relative;
}
.filters {
  display: flex;
  gap: 1.5%;
  margin-bottom: 2%;
  flex-direction: row-reverse;
}
.filter-select {
  padding: 0.5% 1%;
  font-size: 1em;
  font-family: 'Montserrat';
  border: 1px solid lightgray;
  border-radius: 0.25em;
}
.tasks-table {
  width: 100%;
  border-collapse: collapse;
  font-family: 'Montserrat';
  margin-bottom: 2%;
}
.table-header {
  display: flex;
  font-weight: bold;
}
.header-cell, .table-cell {
  padding: 1% 1.5%;
  flex: 1;
  text-align: left;
  min-width: 0;
}
.header-cell:last-child, .table-cell:last-child {
  border-right: none;
}
.table-row {
  display: flex;
  cursor: pointer;
  transition: background-color 0.2s;
}
.table-row:hover {
  background-color: #f9f9f9;
}
.header-cell:nth-child(1), .table-cell:nth-child(1) { flex: 0.5; }
.header-cell:nth-child(2), .table-cell:nth-child(2) { flex: 0.5; }
.header-cell:nth-child(3), .table-cell:nth-child(3) { flex: 2; }
.header-cell:nth-child(4), .table-cell:nth-child(4) { flex: 1; }
.header-cell:nth-child(5), .table-cell:nth-child(5) { flex: 0.7; }
.header-cell:nth-child(6), .table-cell:nth-child(6) { flex: 0.8; }
.header-cell:nth-child(7), .table-cell:nth-child(7) { flex: 0.8; }
</style>