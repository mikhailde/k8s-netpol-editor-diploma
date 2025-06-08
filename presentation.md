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
<style scoped>
  section h2 {
    text-align: left;
    font-size: 50px;
    margin-bottom: 30px;
    padding-bottom: 10px;
    border-bottom: 4px solid #cc0000;
  }
  ul {
    font-size: 28px;
    line-height: 1.6;
  }
  strong {
    color: #cc0000;
  }
</style>

## Актуальность и проблема

**Контекст:** Kubernetes — стандарт индустрии.  
**Инструмент безопасности:** Сетевые политики (Network Policies).

**Главная проблема:** Ручная настройка политик через **YAML** это:
- **Сложно:** Вложенная структура, строгие отступы.
- **Рискованно:** Опечатка в метке или порте — **критическая уязвимость**.
- **Непрозрачно:** Невозможно понять общую картину из десятков файлов.

**Вывод:** Существует острая потребность в инструменте, который делает управление политиками **наглядным, интуитивным и безопасным**.

---
<style scoped>
  section {
    background-image: url('https://raw.githubusercontent.com/mikhailde/k8s-netpol-editor-diploma/main/images/bg-accent.png');
  }
  section h2 {
    font-size: 50px;
    margin-bottom: 40px;
  }
  .columns {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 40px;
    font-size: 24px;
  }
  .columns ul {
    margin: 0;
    padding-left: 25px;
  }
  .columns li {
    margin-bottom: 15px;
  }
  .columns h3 {
    font-size: 30px;
    color: #cc0000;
    border-bottom: 3px solid #cc0000;
    padding-bottom: 5px;
    margin-bottom: 20px;
  }
</style>

## Анализ существующих решений

<div class="columns">
<div>
<h3>Ручные и CLI-инструменты</h3>

- **kubectl:**
  - Полностью ручная работа с YAML.
  - Высокий риск ошибок.
  - Нет визуализации.

- **Calico / Cilium CLI:**
  - Мощный функционал.
  - **Привязка к экосистеме (вендор-лок).**
  - Усложняют стек технологий.

</div>
<div>
<h3>Визуальные инструменты (UI)</h3>

- **Lens / MKE UI:**
  - Часто лишь "оболочка" для YAML.
  - Нет специализированного редактора.

- **Cilium Editor / Sysdig:**
  - Фокус на своей CNI или коммерция.
  - **Не решают проблему для стандартных политик.**

</div>
</div>

---
<style scoped>
  section h2 {
    font-size: 50px;
    margin-bottom: 20px;
  }
  .container {
    display: flex;
    gap: 30px;
    align-items: center;
  }
  .left-pane {
    flex: 1;
  }
  .right-pane {
    flex: 1;
  }
  .right-pane img {
      width: 100%;
      height: auto;
      display: block;
      border: 3px solid #0d2b51;
      border-radius: 10px;
  }
  .left-pane ul {
      margin-top: 0;
      padding-left: 20px;
      font-size: 26px;
      line-height: 1.5;
  }
  .left-pane ul ul {
      padding-left: 30px;
      font-size: 24px;
  }
</style>

## Решение: Визуальный редактор

<div class="container">
<div class="left-pane">

- **Интуитивное проектирование**
  - Drag-and-drop элементов
  - Наглядное создание связей
- **Интерактивная настройка**
  - Редактирование в **Инспекторе**
  - **Валидация** в реальном времени
- **Автоматическая генерация**
  - Корректный **YAML** в один клик
  - Готовые к применению манифесты

</div>
<div class="right-pane">

![width:550px](https://raw.githubusercontent.com/mikhailde/k8s-netpol-editor-diploma/main/images/demo.png)

</div>
</div>

---
<style scoped>
  section h2 {
    font-size: 50px;
    margin-bottom: 25px;
  }
  .tech-grid-final {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 15px 40px;
    max-width: 850px;
  }
  .tech-item {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: flex-start;
    text-align: center;
  }
  .tech-item img {
    height: 75px;
    margin-bottom: 10px;
    object-fit: contain;
  }
  .tech-item h3 {
    margin: 0;
    font-size: 26px;
  }
  .tech-item p {
    font-size: 19px;
    margin: 5px 0 0 0;
    min-height: 40px;
  }
</style>

## Технологический стек

<div class="tech-grid-final">
  <div class="tech-item">
    <img src="https://raw.githubusercontent.com/remojansen/logo.ts/master/ts.png" alt="TypeScript">
    <h3>TypeScript</h3>
    <p>Строгая типизация <br> и надежность кода.</p>
  </div>
  <div class="tech-item">
    <img src="https://upload.wikimedia.org/wikipedia/commons/a/a7/React-icon.svg" alt="React">
    <h3>React</h3>
    <p>Компонентная <br> архитектура UI.</p>
  </div>
  <div class="tech-item">
    <img src="https://vitejs.dev/logo-with-shadow.png" alt="Vite">
    <h3>Vite</h3>
    <p>Современная и быстрая <br> сборка проекта.</p>
  </div>
  <div class="tech-item">
    <img src="https://reactflow.dev/img/logo.svg" alt="ReactFlow">
    <h3>ReactFlow</h3>
    <p>Создание <br> интерактивного холста.</p>
  </div>
  <div class="tech-item">
    <img src="https://raw.githubusercontent.com/pmndrs/zustand/main/examples/starter/src/assets/zustand-mascot.svg" alt="Zustand">
    <h3>Zustand</h3>
    <p>Легковесное управление <br> состоянием.</p>
  </div>
  <div class="tech-item">
    <img src="https://raw.githubusercontent.com/jestjs/jest/main/website/static/img/jest.png" alt="Jest/Playwright">
    <h3>Jest & Playwright</h3>
    <p>Комплексное Unit и E2E <br> тестирование.</p>
  </div>
</div>

---
<style scoped>
  section h2 {
    font-size: 50px;
    margin-bottom: 30px;
  }
  .test-container {
    display: flex;
    flex-direction: column;
    gap: 25px;
    max-width: 80%;
  }
  .test-item {
    border-left: 5px solid #cc0000;
    padding-left: 20px;
  }
  .test-item h3 {
    font-size: 32px;
    margin: 0 0 5px 0;
  }
  .test-item p {
    font-size: 24px;
    margin: 0;
    line-height: 1.4;
  }
</style>

## Обеспечение качества: Комплексное тестирование

<div class="test-container">
  <div class="test-item">
    <h3>Модульное и Интеграционное тестирование (Jest)</h3>
    <p>Проверена корректность бизнес-логики (генерация YAML, валидация) и правильность взаимодействия UI-компонентов.</p>
  </div>
  <div class="test-item">
    <h3>Сквозное тестирование (Playwright)</h3>
    <p>Проверены полные сценарии работы пользователя, эмулируя реальные действия в браузере от начала до конца.</p>
  </div>
  <div class="test-item">
    <h3>Результат</h3>
    <p>Подтверждена стабильность работы прототипа и корректность генерируемых конфигураций для всех тестовых сценариев.</p>
  </div>
</div>

---
<style scoped>
  section {
    background-image: url('https://raw.githubusercontent.com/mikhailde/k8s-netpol-editor-diploma/main/images/bg-accent.png');
  }
  section h2 {
    font-size: 50px;
    margin-bottom: 30px;
  }
  .summary-columns {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 50px;
  }
  .summary-columns h3 {
    font-size: 32px;
    color: #cc0000;
    margin-top: 0;
    border-bottom: 3px solid #cc0000;
    padding-bottom: 10px;
  }
  .summary-columns ul {
    margin: 0;
    padding-left: 25px;
    font-size: 26px;
  }
  .summary-columns li {
    margin-bottom: 15px;
  }
</style>

## Заключение и перспективы

<div class="summary-columns">
<div>

### Достигнутые результаты

- **Цели проекта выполнены:** создан прототип визуального редактора.
- **Проблема решена:** процесс настройки политик упрощен и стал нагляднее.
- **Качество подтверждено:** проведено комплексное тестирование.
- **Практическая значимость:** инструмент готов к использованию администраторами и DevOps-инженерами.

</div>
<div>

### Перспективы развития

- **Реализация импорта** существующих YAML-файлов.
- **Прямая интеграция** с Kubernetes API.
- **Поддержка расширенных политик** (например, для Calico/Cilium).
- **Сохранение и загрузка** проектов (схем).

</div>
</div>

---
<style scoped>
  section {
    background-image: url('https://raw.githubusercontent.com/mikhailde/k8s-netpol-editor-diploma/main/images/bg-conclusion.png');
    padding: 0;
  }
  section.final h2 {
    position: absolute;
    left: 80px;
    font-size: 60px;
    color: #0d2b51;
    font-weight: 600;
    text-align: left;
    line-height: 1.2;
  }
  section.final h2:first-of-type {
    top: 300px;
  }
  section.final h2:last-of-type {
    top: 480px;
  }
</style>

<!-- _class: final -->
<!-- _header: '' -->
<!-- _footer: '' -->
<!-- _paginate: false -->

## Спасибо за внимание!

## Готов ответить <br> на ваши вопросы.