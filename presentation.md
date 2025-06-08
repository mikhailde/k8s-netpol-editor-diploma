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
    align-items: flex-start;
  }
  .left-pane {
    flex: 2;
  }
  .right-pane {
    flex: 3;
    border: 3px solid #0d2b51;
    border-radius: 10px;
    overflow: hidden;
  }
  .right-pane img {
      width: 100%;
      height: auto;
      display: block;
  }
  .left-pane ul {
      margin-top: 0;
      font-size: 26px;
      line-height: 1.5;
  }
</style>

## Решение: Визуальный редактор

<div class="container">
<div class="left-pane">

**Ключевые возможности:**

- **Интуитивное проектирование:**
  - Drag-and-drop элементов.
  - Наглядное создание связей.

- **Интерактивная настройка:**
  - Редактирование свойств в **Инспекторе**.
  - **Валидация в реальном времени** для предотвращения ошибок.

- **Автоматическая генерация:**
  - Корректный **YAML** в один клик.
  - Готовые к применению манифесты.

</div>
<div class="right-pane">

![width:100%](https://raw.githubusercontent.com/mikhailde/k8s-netpol-editor-diploma/main/images/demo.png)

</div>
</div>

---