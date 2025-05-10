<template>
  <div class="container">
    <SidebarMenu />
    <div class="main-content">
      <h1 class="page-title">Задача</h1>
      <nuxt-link to="/tasks" class="return-button">Вернуться</nuxt-link>
      <div class="task-container" v-if="currentTask">
        <div class="task-title-container">
          <h2 class="task-title">{{ currentTask.title }}</h2>
          <span class="task-date">{{ formatDate(currentTask.createdAt) }}</span>
        </div>
        <div class="task-content">{{ currentTask.content }}</div>
        <div class="task-options" v-if="currentTask.options && currentTask.options.length">
          <div v-for="(option, index) in currentTask.options" :key="index" class="option-item">
            <input type="checkbox" :id="'option'+index" v-model="selectedOptions" :value="option">
            <label :for="'option'+index">{{ option }}</label>
          </div>
        </div>
      </div>
      <div class="reply-editor">
        <div class="mini-input-container" v-if="!showFullEditor" @click="showFullEditor = true">
          <img src="/assets/2.png" alt="Аватар" class="user-avatar">
          <div class="mini-input-wrapper">
            <div class="mini-input"></div>
          </div>
          <button class="mini-add-button">Добавить ответ</button>
        </div>
        <div class="answer-editor" v-if="showFullEditor">
          <div class="editor-toolbar">
            <img src="/assets/2.png" alt="Аватар" class="user-avatar">
            <button @click="formatText('bold')" class="toolbar-button" :class="{ active: textBold }" title="Полужирный">
              <svg class="icon" viewBox="0 0 24 24" width="18" height="18">
                <path d="M15.6 10.79c.97-.67 1.65-1.77 1.65-2.79 0-2.26-1.75-4-4-4H7v14h7.04c2.09 0 3.71-1.7 3.71-3.79 0-1.52-.86-2.82-2.15-3.42zM10 6.5h3c.83 0 1.5.67 1.5 1.5s-.67 1.5-1.5 1.5h-3v-3zm3.5 9H10v-3h3.5c.83 0 1.5.67 1.5 1.5s-.67 1.5-1.5 1.5z" fill="currentColor"/>
              </svg>
            </button>
            <button @click="formatText('italic')" class="toolbar-button" :class="{ active: textItalic }" title="Курсив">
              <svg class="icon" viewBox="0 0 24 24" width="18" height="18">
                <path d="M10 4v3h2.21l-3.42 8H6v3h8v-3h-2.21l3.42-8H18V4h-8z" fill="currentColor"/>
              </svg>
            </button>
            <button @click="formatText('underline')" class="toolbar-button" :class="{ active: textUnderline }" title="Подчеркивание">
              <svg class="icon" viewBox="0 0 24 24" width="18" height="18">
                <path d="M12 17c3.31 0 6-2.69 6-6V3h-2.5v8c0 1.93-1.57 3.5-3.5 3.5S8.5 12.93 8.5 11V3H6v8c0 3.31 2.69 6 6 6zm-7 2v2h14v-2H5z" fill="currentColor"/>
              </svg>
            </button>
            <div class="toolbar-separator">|</div>
            <button @click="formatText('quote')" class="toolbar-button" title="Цитата">
              <svg class="icon" viewBox="0 0 24 24" width="18" height="18">
                <path d="M6 17h3l2-4V7H5v6h3zm8 0h3l2-4V7h-6v6h3z" fill="currentColor"/>
              </svg>
            </button>
            <button @click="insertEmoji('😊')" class="toolbar-button" title="Смайлик">
              <span class="icon">☺</span>
            </button>
            <button @click="toggleVisibility" class="toolbar-button" :class="{ active: !showText }" title="Скрыть/показать текст">
              <svg class="icon" viewBox="0 0 24 24" width="18" height="18">
                <path v-if="showText" d="M12 4.5C7 4.5 2.73 7.61 1 12c1.73 4.39 6 7.5 11 7.5s9.27-3.11 11-7.5c-1.73-4.39-6-7.5-11-7.5zM12 17c-2.76 0-5-2.24-5-5s2.24-5 5-5 5 2.24 5 5-2.24 5-5 5zm0-8c-1.66 0-3 1.34-3 3s1.34 3 3 3 3-1.34 3-3-1.34-3-3-3z" fill="currentColor"/>
                <path v-else d="M12 7c2.76 0 5 2.24 5 5 0 .65-.13 1.26-.36 1.83l2.92 2.92c1.51-1.26 2.7-2.89 3.43-4.75-1.73-4.39-6-7.5-11-7.5-1.4 0-2.74.25-3.98.7l2.16 2.16C10.74 7.13 11.35 7 12 7zM2 4.27l2.28 2.28.46.46C3.08 8.3 1.78 10.02 1 12c1.73 4.39 6 7.5 11 7.5 1.55 0 3.03-.3 4.38-.84l.42.42L19.73 22 21 20.73 3.27 3 2 4.27zM7.53 9.8l1.55 1.55c-.05.21-.08.43-.08.65 0 1.66 1.34 3 3 3 .22 0 .44-.03.65-.08l1.55 1.55c-.67.33-1.41.53-2.2.53-2.76 0-5-2.24-5-5 0-.79.2-1.53.53-2.2zm4.31-.78l3.15 3.15.02-.16c0-1.66-1.34-3-3-3l-.17.01z" fill="currentColor"/>
              </svg>
            </button>
            <button @click="formatText('list')" class="toolbar-button" title="Список">
              <svg class="icon" viewBox="0 0 24 24" width="18" height="18">
                <path d="M3 13h2v-2H3v2zm0 4h2v-2H3v2zm0-8h2V7H3v2zm4 4h14v-2H7v2zm0 4h14v-2H7v2zM7 7v2h14V7H7z" fill="currentColor"/>
              </svg>
            </button>
            <div class="toolbar-separator">|</div>
            <select v-model="selectedFont" class="toolbar-select">
              <option value="Arial">Arial</option>
              <option value="Times New Roman">Times New Roman</option>
              <option value="Courier New">Courier New</option>
            </select>
            <select v-model="selectedFontSize" class="toolbar-select">
              <option value="12px">12px</option>
              <option value="14px">14px</option>
              <option value="16px">16px</option>
              <option value="18px">18px</option>
            </select>
            <button @click="clearText" class="toolbar-button danger" title="Очистить">
              <span class="icon-clear">×</span>
            </button>
            <button class="close-editor" @click="showFullEditor = false">&times;</button>
          </div>
          <div class="editor-wrapper">
            <textarea 
              ref="editor" 
              v-model="answer" 
              class="editor-textarea" 
              :style="{ 
                fontFamily: selectedFont,
                fontSize: selectedFontSize,
                textDecoration: textUnderline ? 'underline' : 'none',
                fontStyle: textItalic ? 'italic' : 'normal',
                fontWeight: textBold ? 'bold' : 'normal',
                display: showText ? 'block' : 'none',
                paddingRight: '100px'
              }"
              placeholder="Введите ваш ответ здесь..."></textarea>
            <div v-if="!showText" class="hidden-text-placeholder">
              Текст скрыт
            </div>
            <button @click="submitAnswer" class="add-answer-button">
              Добавить ответ
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import SidebarMenu from '~/components/sidebarmenu.vue'

export default {
  name: 'TaskPage',
  components: {
    SidebarMenu
  },
  data() {
    return {
      answer: '',
      selectedOptions: [],
      selectedFont: 'Arial',
      selectedFontSize: '14px',
      showText: true,
      showFullEditor: false,
      tasks: [
        {
          id: 1,
          title: 'Анатомия сердца',
          content: 'Опишите основные анатомические структуры сердца и их функциональное значение в контексте спортивной медицины. Как изменяется работа сердца у профессиональных спортсменов?',
          options: [
            'Правый желудочек',
            'Левый желудочек',
            'Митральный клапан',
            'Все перечисленные варианты'
          ],
          createdAt: '2023-05-15',
          topic: 'Анатомия'
        },
        {
          id: 2,
          title: 'Фармакология антибиотиков',
          content: 'Проанализируйте применение антибиотиков в спортивной медицине. Какие группы антибиотиков наиболее часто используются при лечении инфекционных заболеваний у спортсменов и почему?',
          options: [
            'Пенициллины - из-за широкого спектра действия',
            'Цефалоспорины - для лечения MRSA инфекций',
            'Макролиды - при аллергии на пенициллины',
            'Фторхинолоны - при инфекциях мочевыводящих путей'
          ],
          createdAt: '2023-06-20',
          topic: 'Фармакология'
        },
        {
          id: 3,
          title: 'Неврологический осмотр',
          content: 'Разработайте алгоритм неврологического осмотра спортсмена после черепно-мозговой травмы. Какие специфические тесты следует включить для оценки возможности возвращения к тренировкам?',
          options: [
            'Тест на равновесие и координацию',
            'Оценка когнитивных функций',
            'Проверка рефлексов и мышечной силы',
            'Все перечисленные варианты'
          ],
          createdAt: '2023-07-10',
          topic: 'Неврология'
        }
      ],
      currentTask: null,
      textBold: false,
      textItalic: false,
      textUnderline: false
    }
  },
  created() {
    this.loadTask();
  },
  methods: {
    loadTask() {
      const taskId = this.$route.params.id;
      this.currentTask = this.tasks.find(task => task.id.toString() === taskId) || null;
    },
    formatText(type) {
      const textarea = this.$refs.editor;
      const start = textarea.selectionStart;
      const end = textarea.selectionEnd;
      
      switch(type) {
        case 'bold':
          this.textBold = !this.textBold;
          break;
        case 'italic':
          this.textItalic = !this.textItalic;
          break;
        case 'underline':
          this.textUnderline = !this.textUnderline;
          break;
        case 'quote':
          const selectedText = this.answer.substring(start, end);
          const quotedText = `"${selectedText}"`;
          this.answer = this.answer.substring(0, start) + quotedText + this.answer.substring(end);
          break;
        case 'list':
          const textLines = this.answer.substring(start, end).split('\n');
          const listedText = textLines.map(line => line ? `- ${line}` : '').join('\n');
          this.answer = this.answer.substring(0, start) + listedText + this.answer.substring(end);
          break;
      }
      textarea.focus();
      setTimeout(() => {
        textarea.selectionStart = start;
        textarea.selectionEnd = end;
      }, 0);
    },
    insertEmoji(emoji) {
      const textarea = this.$refs.editor;
      const start = textarea.selectionStart;
      this.answer = this.answer.substring(0, start) + emoji + this.answer.substring(start);
      textarea.focus();
      setTimeout(() => {
        textarea.selectionStart = start + emoji.length;
        textarea.selectionEnd = start + emoji.length;
      }, 0);
    },
    toggleVisibility() {
      this.showText = !this.showText;
    },
    clearText() {
      this.answer = '';
      this.textBold = false;
      this.textItalic = false;
      this.textUnderline = false;
    },
    submitAnswer() {
      const response = {
        text: this.answer,
        selectedOptions: this.selectedOptions
      };
      alert(`Ответ отправлен:\nТекст: ${response.text}\nВыбранные варианты: ${response.selectedOptions.join(', ') || 'нет'}`);
      this.answer = '';
      this.selectedOptions = [];
      this.showFullEditor = false;
    },
    formatDate(dateString) {
      const options = { year: 'numeric', month: 'long', day: 'numeric' };
      return new Date(dateString).toLocaleDateString('ru-RU', options);
    }
  },
  watch: {
    '$route.params.id': {
      handler() {
        this.loadTask();
      },
      immediate: true
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
  display: flex;
  flex-direction: column;
}
.page-title {
  margin-bottom: 1%;
  font-size: 1.5em;
  font-weight: normal;
  font-family: 'Montserrat Medium';
}
.return-button {
  display: flex;
  align-items: center;
  padding: 0.5% 1%;
  background-color: #f0f2f5;
  border: none;
  border-radius: 0.5em;
  cursor: pointer;
  text-decoration: none;
  font-size: 0.9em;
  transition: background-color 0.2s;
  margin-bottom: 1%;
  color: black;
  font-family: 'Montserrat';
  width: fit-content;
}
.return-button:hover {
  background-color: #e4e6eb;
}
.task-container {
  flex: 1;
  overflow-y: auto;
  padding-bottom: 20px;
}
.task-title-container {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 1%;
}
.task-title {
  font-family: 'Montserrat Medium';
  font-weight: normal;
  font-size: 1.1em;
}
.task-date {
  font-size: 1em;
  font-family: 'Montserrat Medium';
}
.task-content {
  margin-bottom: 2%;
  font-family: 'Montserrat';
  font-size: 1em;
  line-height: 1.6;
}
.task-options {
  margin: 1% 0;
}
.option-item {
  margin: 1% 0;
  display: flex;
  align-items: center;
  font-family: 'Montserrat';
  font-size: 1em;
}
.option-item input {
  margin-right: 1%;
  width: 18px;
  height: 18px;
  accent-color: #4ba285;
}
.reply-editor {
  margin-top: -4vh;
  background: white;
  border-radius: 0.5em;
}
.mini-input-container {
  display: flex;
  padding: 1%;
  border: 1px solid #e4e6eb;
  border-radius: 1em;
  cursor: pointer;
  background-color: white;
}
.mini-input-wrapper {
  flex: 1;
  background-color: #f0f2f5;
  border-radius: 0.5em;
  padding: 0.5% 1%;
  margin: 0 1%;
}
.user-avatar {
  width: 32px;
  height: 32px;
  border-radius: 50%;
  margin-right: 1%;
  object-fit: cover;
}
.mini-input {
  color: #777;
  font-family: 'Montserrat';
  font-size: 0.9em;
}
.mini-add-button {
  padding: 0.75% 1.5%;
  background: #4ba285;
  color: white;
  border: none;
  border-radius: 0.5em;
  cursor: pointer;
  font-size: 0.9em;
  font-family: 'Montserrat';
  transition: background-color 0.2s;
  white-space: nowrap;
}
.mini-add-button:hover {
  background: #3a826a;
}
.answer-editor {
  border: 1px solid #e4e6eb;
  border-radius: 1em;
  overflow: hidden;
}
.editor-toolbar {
  display: flex;
  gap: 1%;
  padding: 1%;
  align-items: center;
  flex-wrap: wrap;
}
.editor-toolbar .user-avatar {
  margin-right: 0.5%;
}
.toolbar-button {
  width: 32px;
  height: 32px;
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: transparent;
  border: none;
  border-radius: 0.25em;
  cursor: pointer;
  transition: all 0.2s;
}
.toolbar-button:hover {
  background-color: #f0f2f5;
}
.toolbar-button.active {
  background-color: #e4e6eb;
}
.toolbar-button.danger:hover {
  background-color: #ffebee;
  color: #f44336;
}
.toolbar-separator {
  color: #b0bec5;
  font-size: 1em;
  margin: 0 1%;
}
.toolbar-select {
  padding: 0.5%;
  border: 1px solid #e4e6eb;
  border-radius: 0.5em;
  background-color: white;
  font-size: 0.8em;
  cursor: pointer;
}
.editor-wrapper {
  position: relative;
}
.editor-textarea {
  width: 100%;
  min-height: 200px;
  padding: 1%;
  padding-right: 1%;
  border: none;
  font-family: inherit;
  resize: vertical;
  font-size: inherit;
  line-height: 1.6;
  outline: none;
  background-color: white;
}
.hidden-text-placeholder {
  padding: 1%;
  font-style: 'Montserrat Medium';
  background-color: #f8f9fa;
}
.add-answer-button {
  position: absolute;
  right: 1%;
  bottom: 5%;
  padding: 0.75% 1.5%;
  background: #4ba285;
  color: white;
  border: none;
  border-radius: 0.5em;
  cursor: pointer;
  font-size: 0.9em;
  font-family: 'Montserrat';
  transition: background-color 0.2s;
}
.add-answer-button:hover {
  background: #3a826a;
}
.close-editor {
  background: none;
  border: none;
  font-size: 1.5em;
  cursor: pointer;
  color: #777;
  padding: 0 1%;
  margin-left: auto;
}
.close-editor:hover {
  color: #333;
}
.icon-bold { font-weight: bold; }
.icon-italic { font-style: italic; }
.icon-underline { text-decoration: underline; }
.icon { font-size: 1.5em; }
.icon-clear { font-size: 1.5em; }
</style>