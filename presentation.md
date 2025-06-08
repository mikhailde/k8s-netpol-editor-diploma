---
marp: true
theme: default
style: |
  section {
    background-image: url('https://raw.githubusercontent.com/mikhailde/k8s-netpol-editor-diploma/main/images/bg-main.png');
    background-size: cover;
    color: #0d2b51;
    font-size: 30px;
    padding: 50px 70px;
  }
  section.title {
    background-image: url('https://raw.githubusercontent.com/mikhailde/k8s-netpol-editor-diploma/main/images/bg-title.png');
    color: #ffffff;
    padding: 0;
  }
  section.title h1, section.title p {
    position: absolute;
    margin: 0;
    padding: 0;
    font-weight: 400;
  }
  section.title h1 {
    top: 280px;
    left: 40px;
    font-size: 60px;
    line-height: 1.2;
    text-align: left;
    color: #ffffff;
    font-weight: 600;
  }
  section.title p {
    bottom: 10px;
    right: 20px;
    font-size: 24px;
    line-height: 1.5;
    text-align: left;
  }
  b {
    font-weight: 600;
  }
---

<!-- _class: title -->
<!-- _header: '' -->
<!-- _footer: '' -->
<!-- _paginate: false -->

# Разработка визуального редактора <br> политик сетевой безопасности для <br> Kubernetes

<p>
  <b>Щанников Михаил Викторович</b><br>
  Группа: К0411-22<br>
  Специальность: 10.02.04 Обеспечение информационной<br>безопасности телекоммуникационных систем
</p>

---
<style>
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap');

:root {
  --font-main: 'Inter', sans-serif;
  --color-dark-blue: #0d2b51;
  --color-white: #ffffff;
  --color-accent-red: #005fB9;
  --color-bg-light: #f8f9fa;
  --color-border: #dee2e6;
}

section:not(.title) {
  font-family: var(--font-main);
  background-image: url('https://raw.githubusercontent.com/mikhailde/k8s-netpol-editor-diploma/main/images/bg-main.png');
  background-size: cover;
  color: var(--color-dark-blue);
  font-size: 28px;
  line-height: 1.6;
  padding: 50px 70px;
}

section:not(.title) h2 {
  font-family: var(--font-main);
  font-weight: 700;
  font-size: 48px;
  text-align: left;
  margin-bottom: 30px;
  padding-bottom: 10px;
  border-bottom: 4px solid var(--color-accent-red);
  color: var(--color-dark-blue);
}

section:not(.title) h3 {
  font-family: var(--font-main);
  font-weight: 700;
  font-size: 32px;
  color: var(--color-accent-red);
}

section:not(.title) ul {
  list-style: none;
  padding-left: 0;
}

section:not(.title) li {
  padding-left: 1.5em;
  position: relative;
  margin-bottom: 12px;
}

section:not(.title) li::before {
  content: '■';
  position: absolute;
  left: 0;
  color: var(--color-accent-red);
  font-size: 0.8em;
  top: 0.2em;
}

section:not(.title) b, section:not(.title) strong {
  font-weight: 600;
  color: var(--color-accent-red);
}
</style>
<style scoped>
.problem-layout {
  display: grid;
  grid-template-columns: 1fr 1.5fr;
  gap: 50px;
  align-items: start;
}
.problem-layout h3 {
  border-bottom: 3px solid var(--color-accent-red);
  padding-bottom: 8px;
  margin-bottom: 20px;
  margin-top: 0;
}
.problem-layout ul {
    font-size: 26px;
}
p:last-of-type {
  margin-right: 220px;
  margin-top: 25px;
}
</style>

## Актуальность и проблема

**Контекст:** Kubernetes — стандарт индустрии.  
**Инструмент безопасности:** Сетевые политики (Network Policies).

**Главная проблема:** Ручная настройка политик через **YAML** это:
- **Сложно:** Вложенная структура, строгие отступы.
- **Рискованно:** Опечатка в метке или порте — критическая уязвимость.
- **Непрозрачно:** Невозможно понять общую картину из десятков файлов.

**Вывод:** Существует острая потребность в инструменте, который делает управление политиками наглядным, интуитивным и безопасным.

---
<style scoped>
.solutions-layout {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 50px;
  align-items: start;
}
.solutions-layout > div {
  background-color: rgba(255, 255, 255, 0.75);
  padding: 25px;
  border-radius: 8px;
  border: 1px solid #0000001a;
  box-shadow: 0 4px 6px #0000001a;
}
.solutions-layout h3 {
  margin-top: 0;
  padding-bottom: 8px;
  margin-bottom: 20px;
  border-bottom: 3px solid var(--color-accent-red);
}
.tool-block {
    margin-bottom: 25px;
}
.tool-block:last-child {
    margin-bottom: 0;
}
.tool-block p {
    margin: 5px 0 0 15px;
    font-size: 25px;
    line-height: 1.4;
}
</style>

## Анализ существующих решений

<div class="solutions-layout">
<div>
<h3>Ручные и CLI-инструменты</h3>
<div class="tool-block">
  <strong>kubectl:</strong>
  <p>Полностью ручная работа с YAML.</p>
</div>
<div class="tool-block">
  <strong>Calico / Cilium CLI:</strong>
  <p>Привязка к экосистеме (вендор-лок).</p>
</div>

</div>
<div>
<h3>Визуальные инструменты (UI)</h3>
<div class="tool-block">
  <strong>Lens / MKE UI:</strong>
  <p>Часто лишь "оболочка" для YAML.</p>
</div>
<div class="tool-block">
  <strong>Cilium Editor / Sysdig:</strong>
  <p>Не решают проблему для стандартных политик.</p>
</div>

</div>
</div>

---
<style scoped>
.solution-layout-asymmetric {
  display: grid;
  grid-template-columns: 0.7fr 1.3fr;
  gap: 40px;
  align-items: start;
  height: 100%;
}
.left-pane-compact .feature-block {
  margin-bottom: 30px;
}
.left-pane-compact .feature-block:last-child {
  margin-bottom: 0;
}
.left-pane-compact h3 {
  margin: 0 0 8px 0;
  font-size: 26px;
  border: none;
  padding: 0;
  display: flex;
  align-items: center;
}
.left-pane-compact h3::before {
  content: '■';
  color: var(--color-accent-red);
  margin-right: 12px;
  font-size: 0.8em;
}
.left-pane-compact p {
  margin: 0 0 5px 28px;
  font-size: 22px;
  line-height: 1.3;
}
.right-pane-large img {
  width: 100%;
  max-width: none;
  height: auto;
  border-radius: 12px;
  box-shadow: 0 8px 25px rgba(0, 0, 0, 0.2);
  border: 1px solid rgba(0,0,0,0.2);
}
</style>

## Решение: Визуальный редактор

<div class="solution-layout-asymmetric">
<div class="left-pane-compact">

<div class="feature-block">
  <h3>Интуитивное проектирование</h3>
  <p>› Drag-and-drop</p>
  <p>› Визуальные связи</p>
</div>

<div class="feature-block">
  <h3>Интерактивная настройка</h3>
  <p>› Редактор в Инспекторе</p>
  <p>› Валидация в реальном времени</p>
</div>

<div class="feature-block">
  <h3>Автоматическая генерация</h3>
  <p>› Корректный YAML в один клик</p>
</div>

</div>
<div class="right-pane-large">

![img_large](https://raw.githubusercontent.com/mikhailde/k8s-netpol-editor-diploma/main/images/demo.png)

</div>
</div>

---
<style scoped>
.tech-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 20px;
  max-width: 900px;
  margin: 20px 0 0 0;
}
.tech-item {
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
  background-color: rgba(255, 255, 255, 0.75);
  padding: 15px;
  border-radius: 12px;
  border: 1px solid #0000001a;
  box-shadow: 0 4px 6px #0000001a;
  transition: transform 0.2s ease-in-out, box-shadow 0.2s ease-in-out;
}
.tech-item:hover {
  transform: translateY(-5px);
  box-shadow: 0 8px 12px #0000002a;
}
.tech-item img {
  height: 60px;
  margin-bottom: 10px;
  object-fit: contain;
}
.tech-item h3 {
  margin: 0;
  font-size: 24px;
  border: none;
  padding: 0;
}
.tech-item p {
  font-size: 18px;
  margin: 5px 0 0 0;
  line-height: 1.3;
  min-height: 48px;
}
</style>

## Технологический стек

<div class="tech-grid">
  <div class="tech-item">
    <img src="https://raw.githubusercontent.com/remojansen/logo.ts/master/ts.png" alt="TypeScript">
    <h3>TypeScript</h3>
    <p>Строгая типизация и надежность кода.</p>
  </div>
  <div class="tech-item">
    <img src="https://upload.wikimedia.org/wikipedia/commons/a/a7/React-icon.svg" alt="React">
    <h3>React</h3>
    <p>Компонентная архитектура UI.</p>
  </div>
    <div class="tech-item">
    <img src="https://reactflow.dev/img/logo.svg" alt="ReactFlow">
    <h3>ReactFlow</h3>
    <p>Создание интерактивного холста.</p>
  </div>
  <div class="tech-item">
    <img src="https://vitejs.dev/logo-with-shadow.png" alt="Vite">
    <h3>Vite</h3>
    <p>Современная и быстрая сборка проекта.</p>
  </div>
  <div class="tech-item">
    <img src="https://raw.githubusercontent.com/pmndrs/zustand/main/examples/starter/src/assets/zustand-mascot.svg" alt="Zustand">
    <h3>Zustand</h3>
    <p>Легковесное управление состоянием.</p>
  </div>
  <div class="tech-item">
    <img src="https://raw.githubusercontent.com/jestjs/jest/main/website/static/img/jest.png" alt="Jest/Playwright">
    <h3>Jest & Playwright</h3>
    <p>Комплексное Unit и E2E тестирование.</p>
  </div>
</div>

---
<style scoped>
.test-container {
  max-width: 90%;
  margin-top: 20px;
}
.test-item {
  position: relative;
  padding-left: 35px;
  margin-bottom: 35px;
}
.test-item::before {
  content: '';
  position: absolute;
  left: 0;
  top: 5px;
  width: 25px;
  height: 25px;
  background-size: contain;
  background-repeat: no-repeat;
  background-position: center;
}
.test-item.unit::before {
  background-image: url('https://api.iconify.design/bx:bx-test-tube.svg?color=%23d7263d');
}
.test-item.e2e::before {
  background-image: url('https://api.iconify.design/bx:bx-sitemap.svg?color=%23d7263d');
}
.test-item.result::before {
  background-image: url('https://api.iconify.design/bx:bxs-check-shield.svg?color=%23d7263d');
}
.test-item h3 {
  font-size: 30px;
  margin: 0 0 5px 0;
  border: none;
  padding: 0;
}
.test-item p {
  font-size: 24px;
  margin: 0;
  line-height: 1.4;
}
</style>

## Обеспечение качества: Комплексное тестирование

<div class="test-container">
  <div class="test-item unit">
    <h3>Модульное и Интеграционное тестирование (Jest)</h3>
    <p>Проверена корректность бизнес-логики (генерация YAML, валидация) и правильность взаимодействия UI-компонентов.</p>
  </div>
  <div class="test-item e2e">
    <h3>Сквозное тестирование (Playwright)</h3>
    <p>Проверены полные сценарии работы пользователя, эмулируя реальные действия в браузере от начала до конца.</p>
  </div>
  <div class="test-item result">
    <h3>Результат</h3>
    <p>Подтверждена стабильность работы прототипа и корректность генерируемых конфигураций для всех тестовых сценариев.</p>
  </div>
</div>

---
<style scoped>
.summary-layout {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 50px;
  align-items: start;
  margin-top: 20px;
}
.summary-layout > div {
  background-color: rgba(255, 255, 255, 0.75);
  padding: 25px;
  border-radius: 8px;
  border: 1px solid #0000001a;
  box-shadow: 0 4px 6px #0000001a;
}
.summary-layout h3 {
  margin-top: 0;
  padding-bottom: 8px;
  margin-bottom: 20px;
  border-bottom: 3px solid var(--color-accent-red);
}
.summary-layout li {
  font-size: 24px;
}
.summary-layout li::before {
  top: 0.15em;
}
.results li::before {
  content: '✓';
  color: #28a745;
}
.perspectives li::before {
  content: '›';
  color: var(--color-accent-red);
}
</style>

## Заключение и перспективы

<div class="summary-layout">
<div class="results">
<h3>Достигнутые результаты</h3>
<ul>
  <li><strong>Цели проекта выполнены:</strong> создан прототип.</li>
  <li><strong>Проблема решена:</strong> процесс упрощен.</li>
  <li><strong>Качество подтверждено:</strong> тесты пройдены.</li>
  <li><strong>Практическая значимость:</strong> готов к работе.</li>
</ul>

</div>
<div class="perspectives">
<h3>Перспективы развития</h3>
<ul>
  <li>Реализация импорта YAML-файлов.</li>
  <li>Прямая интеграция с Kubernetes API.</li>
  <li>Поддержка расширенных политик.</li>
  <li>Сохранение и загрузка проектов.</li>
</ul>

</div>
</div>

---
<style scoped>
section.final {
  background-image: url('https://raw.githubusercontent.com/mikhailde/k8s-netpol-editor-diploma/main/images/bg-conclusion.png');
  padding: 0;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: flex-start;
  padding-left: 80px;
}

section.final h2, section.final h3 {
  color: var(--color-dark-blue);
  text-align: left;
  line-height: 1.2;
  margin: 0;
  font-weight: 700;
}

section.final h2 {
  font-size: 60px;
  margin-bottom: 25px;
}

section.final h3 {
  font-size: 52px;
  font-weight: 600;
}
</style>

<!-- _class: final -->
<!-- _header: '' -->
<!-- _footer: '' -->
<!-- _paginate: false -->

<h2>Спасибо за внимание!</h2>
<h3>Готов ответить <br> на ваши вопросы.</h3>