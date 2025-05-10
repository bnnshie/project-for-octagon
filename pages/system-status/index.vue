<template>
  <div class="container">
    <SidebarMenu />
    <div class="main-content">
      <h1 class="page-title">Состояние системы</h1>
      <div class="title-underline"></div>
      <div class="page-description">
        <p>
          На этой странице отображается статус выполнения задач в системе.
        </p>
      </div>
      <div class="filters">
        <select v-model="filters.status" class="filter-select">
          <option value="all">Все статусы</option>
          <option value="accepted">Принятые</option>
          <option value="rejected">Отклоненные</option>
        </select>
        <select v-model="filters.author" class="filter-select">
          <option value="all">Все авторы</option>
          <option v-for="author in uniqueauthors" :key="author" :value="author">{{ author }}</option>
        </select>
      </div>
      <div class="tasks-table">
        <div class="table-header">
          <div class="header-cell">ID</div>
          <div class="header-cell">Дата</div>
          <div class="header-cell">№ Задачи</div>
          <div class="header-cell">Автор решения</div>
          <div class="header-cell spacer"></div>
          <div class="header-cell">Статус ответа</div>
        </div>
        <div 
          v-for="item in filteredItems" 
          :key="item.id" 
          class="table-row">
          <div class="table-cell">{{ item.id }}</div>
          <div class="table-cell">{{ item.date }}</div>
          <div class="table-cell">{{ item.taskId }}</div>
          <div class="table-cell">{{ item.author }}</div>
          <div class="table-cell spacer"></div>
          <div class="table-cell">{{ item.status === 'accepted' ? 'Принято' : 'Неверный ответ' }}</div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import SidebarMenu from '~/components/sidebarmenu.vue'
export default {
  name: 'SystemStatus',
  components: {
    SidebarMenu
  },
  data() {
    return {
      statusItems: [
        {
          id: 1,
          date: '15.05.2023',
          taskId: 24,
          author: 'Иванов Иван',
          status: 'accepted'
        },
        {
          id: 2,
          date: '20.06.2023',
          taskId: 48,
          author: 'Петров Петр',
          status: 'rejected'
        },
        {
          id: 3,
          date: '10.07.2023',
          taskId: 72,
          author: 'Сидоров Сергей',
          status: 'accepted'
        }
      ],
      filters: {
        status: 'all',
        author: 'all'
      }
    }
  },
  computed: {
    uniqueauthors() {
      return [...new Set(this.statusItems.map(item => item.author))]
    },
    filteredItems() {
      return this.statusItems.filter(item => {
        if (this.filters.status !== 'all' && item.status !== this.filters.status) return false
        if (this.filters.author !== 'all' && item.author !== this.filters.author) return false
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
  margin-bottom: 2%;
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
  overflow: hidden;
  font-family: 'Montserrat';
}
.table-header {
  display: flex;
  font-weight: bold;
}
.header-cell, .table-cell {
  padding: 1% 1.5%;
  text-align: left;
}
.header-cell:nth-child(1),
.table-cell:nth-child(1) {
  flex: 0.5;
}
.header-cell:nth-child(2),
.table-cell:nth-child(2) {
  flex: 0.5;
  text-align: center;
}
.header-cell:nth-child(3),
.table-cell:nth-child(3) {
  flex: 1;
  text-align: center;
}
.header-cell:nth-child(4),
.table-cell:nth-child(4) {
  flex: 2;
}
.header-cell.spacer,
.table-cell.spacer {
  flex: 0.5;
}
.header-cell:nth-child(6),
.table-cell:nth-child(6) {
  flex: 1;
}
.table-row {
  display: flex;
  cursor: pointer;
  transition: background-color 0.2s;
}
.table-row:hover {
  background-color: #f9f9f9;
}
</style>