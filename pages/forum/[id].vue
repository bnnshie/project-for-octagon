<template>
  <div class="container">
    <SidebarMenu />
    <div class="main-content">
      <h1 class="page-title">{{ currenttopic.title }}</h1>
      <nuxt-link to="/forum" class="return-button">Вернуться</nuxt-link>
      <div class="topic-container">
        <div class="topic-post">
          <div class="post-header">
            <img src="/assets/2.png" alt="Аватар" class="user-avatar">
            <div class="post-author">{{ currenttopic.author }}</div>
            <div class="post-date">{{ formatDate(currenttopic.date) }}</div>
          </div>
          <div class="post-content">
            {{ currenttopic.content }}
          </div>
        </div>
        <div class="replies">
          <div 
            v-for="(reply, index) in currenttopic.replies" 
            :key="reply.id" 
            class="reply">
            <div class="post-header">
              <div class="avatar-wrapper">
                <svg v-if="index === 0" class="reply-arrow" width="20" height="14" viewBox="0 0 14 10" fill="none" xmlns="http://www.w3.org/2000/svg">
                  <path d="M0.125 4.5625C0.125 5.875 0.68964 6.74387 1.49955 7.16475C2.30945 7.5865 3.28081 7.625 4.20894 7.625H11.7434V6.75H4.20982C3.31698 6.75 2.46737 6.68 1.91067 6.39037C1.35397 6.10075 1.00725 5.65625 1.00725 4.5625V0.625H0.125V4.5625Z" fill="#808080"/>
                  <path d="M9.82983 5L9.8316 9.375C10.5021 9.0705 11.1859 8.73012 11.8811 8.35475C12.5701 7.97762 13.2344 7.58825 13.875 7.1875C13.2344 6.79462 12.5701 6.40963 11.8811 6.0325C11.2083 5.6679 10.5245 5.32358 9.83072 5H9.82983Z" fill="#808080"/>
                </svg>
                <img src="/assets/2.png" alt="Аватар" class="user-avatar">
              </div>
              <div class="post-author">{{ reply.author }}</div>
              <div class="post-date">{{ formatDate(reply.date) }}</div>
            </div>
            <div class="post-content">
              {{ reply.content }}
            </div>
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
              v-model="replyText" 
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
            <button @click="submitReply" class="add-answer-button">
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
  name: 'ForumTopicPage',
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
          replies: [
            {
              id: 1,
              author: 'Петрова М.',
              date: '2023-03-16',
              content: 'В нашей практике хорошо зарекомендовали себя комбинированные тесты (ПЦР + антиген). При значениях Ct > 35 рекомендуем повторное тестирование через 48 часов.'
            },
            {
              id: 2,
              author: 'Козлов Д.',
              date: '2023-03-17',
              content: 'Важно учитывать клиническую картину. У нас были случаи, когда при отрицательных ПЦР-тестах КТ показывала типичные изменения.'
            }
          ]
        },
        {
          id: 2,
          title: 'Реабилитация после инсульта',
          date: '2023-03-10',
          author: 'Козлова Е.',
          content: 'Какие современные методики нейрореабилитации вы используете в своей практике? Особый интерес представляют кейсы ранней мобилизации пациентов с ишемическим инсультом. Как вы оцениваете эффективность различных подходов к восстановлению когнитивных функций?',
          replies: [
            {
              id: 1,
              author: 'Иванов С.',
              date: '2023-03-11',
              content: 'В нашем центре успешно применяем метод зеркальной терапии в сочетании с БОС-тренингом. Первые результаты заметны уже через 2-3 недели.'
            },
            {
              id: 2,
              author: 'Сидорова Н.',
              date: '2023-03-12',
              content: 'Хорошие результаты показывает ранняя вертикализация с использованием роботизированных систем. Особенно для пациентов с гемипарезом.'
            }
          ]
        },
        {
          id: 3,
          title: 'Педиатрическая фармакология',
          date: '2023-03-05',
          author: 'Васильев О.',
          content: 'Обсуждение особенностей дозирования лекарственных препаратов у детей разных возрастных групп. Какие формулы пересчета доз из взрослых в детские вы считаете наиболее корректными? Проблемы выбора лекарственных форм для детей раннего возраста.',
          replies: [
            {
              id: 1,
              author: 'Михайлова Т.',
              date: '2023-03-06',
              content: 'Для расчета доз у детей до 12 лет предпочитаю использовать формулу Кларка (по массе тела). Для подростков - уже ближе к взрослым дозировкам.'
            }
          ]
        },
        {
          id: 4,
          title: 'Эндопротезирование суставов',
          date: '2023-02-28',
          author: 'Николаев Д.',
          content: 'Сравнительный анализ различных типов эндопротезов тазобедренного сустава. Какие критерии вы используете при выборе протеза? Особенности послеоперационного ведения пациентов с сопутствующим остеопорозом. Опыт применения индивидуальных протезов.',
          replies: [
            {
              id: 1,
              author: 'Федоров П.',
              date: '2023-03-01',
              content: 'При остеопорозе предпочитаем цементные протезы с усиленной ножкой. Обязательно дополняем терапию бисфосфонатами и контролем уровня кальция.'
            },
            {
              id: 2,
              author: 'Громова А.',
              date: '2023-03-03',
              content: 'Для активных пациентов моложе 60 лет выбираем бесцементные протезы с пористым покрытием. Хорошие долгосрочные результаты.'
            }
          ]
        },
        {
          id: 5,
          title: 'Аллергология в практике',
          date: '2023-02-20',
          author: 'Петрова М.',
          content: 'Современные подходы к диагностике и лечению поллинозов. Эффективность различных схем АСИТ. Как вы решаете проблему перекрестной аллергии? Интересны клинические случаи нестандартных аллергических реакций на лекарственные препараты.',
          replies: [
            {
              id: 1,
              author: 'Белова К.',
              date: '2023-02-22',
              content: 'Для пациентов с поливалентной аллергией рекомендуем молекулярную диагностику. Это позволяет точнее подобрать состав для АСИТ.'
            }
          ]
        },
        {
          id: 6,
          title: 'Цифровая медицина',
          date: '2023-02-15',
          author: 'Иванова Т.',
          content: 'Обмен опытом использования телемедицинских технологий в повседневной практике. Какие платформы вы считаете наиболее удобными для дистанционных консультаций? Правовые аспекты телемедицины. Перспективы внедрения ИИ в диагностические процессы.',
          replies: [
            {
              id: 1,
              author: 'Соколов В.',
              date: '2023-02-16',
              content: 'Используем платформу "Доктор рядом" для консультаций. Удобный интерфейс, интеграция с ЕМИАС. Юридически все оформляется как заочная консультация.'
            },
            {
              id: 2,
              author: 'Кузнецова Л.',
              date: '2023-02-18',
              content: 'ИИ-алгоритмы уже помогают в анализе рентгеновских снимков и ЭКГ. Но окончательное решение всегда за врачом.'
            }
          ]
        }
      ],
      replyText: '',
      selectedFont: 'Arial',
      selectedFontSize: '14px',
      showText: true,
      showFullEditor: false,
      textBold: false,
      textItalic: false,
      textUnderline: false
    }
  },
  computed: {
    currenttopic() {
      const topicId = parseInt(this.$route.params.id)
      return this.topics.find(topic => topic.id === topicId) || {}
    }
  },
  methods: {
    formatDate(dateString) {
      const options = { day: 'numeric', month: 'long' }
      return new Date(dateString).toLocaleDateString('ru-RU', options)
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
          const selectedText = this.replyText.substring(start, end);
          const quotedText = `"${selectedText}"`;
          this.replyText = this.replyText.substring(0, start) + quotedText + this.replyText.substring(end);
          break;
        case 'list':
          const textLines = this.replyText.substring(start, end).split('\n');
          const listedText = textLines.map(line => line ? `- ${line}` : '').join('\n');
          this.replyText = this.replyText.substring(0, start) + listedText + this.replyText.substring(end);
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
      this.replyText = this.replyText.substring(0, start) + emoji + this.replyText.substring(start);
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
      this.replyText = '';
      this.textBold = false;
      this.textItalic = false;
      this.textUnderline = false;
    },
    submitReply() {
      if (this.replyText.trim()) {
        this.currenttopic.replies.push({
          id: this.currenttopic.replies.length + 1,
          author: 'Вы',
          date: new Date().toISOString().split('T')[0],
          content: this.replyText
        });
        this.replyText = '';
        this.showFullEditor = false;
      }
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
  display: inline-flex;
  align-items: center;
  padding: 0.5% 1%;
  background-color: #f0f2f5;
  border: none;
  border-radius: 0.5em;
  cursor: pointer;
  text-decoration: none;
  font-size: 0.9em;
  transition: background-color 0.2s;
  width: fit-content;
  color: black;
  font-family: 'Montserrat';
}
.return-button:hover {
  background-color: #e4e6eb;
}
.topic-container {
  flex: 1;
  overflow-y: auto;
}
.topic-post {
  background: white;
  border-radius: 0.5em;
  padding: 1.5%;
  margin-bottom: 2%;
}
.topic-post .post-author{
  margin-left: 0.75%;
}
.reply {
  background: white;
  border-radius: 0.5em;
  padding: 1.5%;
  margin: 1.5% 0 1.5% 3%;
}
.post-header {
  display: flex;
  align-items: center;
  margin-bottom: 1.5%;
}
.avatar-wrapper {
  display: flex;
  align-items: center;
  position: relative;
  margin-right: 0.5vw;
}
.reply-arrow {
  position: absolute;
  left: -25px;
  top: 10px;
  transform: translateY(-50%);
}
.user-avatar {
  width: 32px;
  height: 32px;
  border-radius: 50%;
  object-fit: cover;
}
.post-author {
  font-family: 'Montserrat Medium';
  font-size: 0.95em;
  margin-right: 2%;
}
.post-date {
  font-size: 0.85em;
  margin-left: auto;
  font-family: 'Montserrat Medium';
}
.post-content {
  font-family: 'Montserrat';
  line-height: 1.6;
}
.replies {
  margin-top: 3%;
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