<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Flash Cards</title>

  <style>
    :root {
      --primary: #6366f1;
      --primary-dark: #4f46e5;
      --bg: #f3f4f6;
      --card: #ffffff;
      --text: #111827;
      --muted: #6b7280;
      --danger: #ef4444;
      --success: #10b981;
      --border: #e5e7eb;
    }

    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
      font-family: Arial, sans-serif;
    }

    body {
      background: var(--bg);
      color: var(--text);
      min-height: 100vh;
    }

    header {
      background: var(--primary);
      color: white;
      padding: 18px 5%;
      display: flex;
      justify-content: space-between;
      align-items: center;
      gap: 15px;
      flex-wrap: wrap;
    }

    header h1 {
      font-size: 24px;
    }

    button,
    input,
    textarea {
      font: inherit;
    }

    button {
      border: none;
      cursor: pointer;
      border-radius: 8px;
      padding: 10px 15px;
      transition: 0.2s;
    }

    button:hover {
      transform: translateY(-1px);
      opacity: 0.9;
    }

    .language-btn {
      background: white;
      color: var(--primary);
      font-weight: bold;
    }

    .layout {
      max-width: 1250px;
      margin: 25px auto;
      padding: 0 20px;
      display: grid;
      grid-template-columns: 280px 1fr;
      gap: 22px;
    }

    .sidebar,
    .content-box {
      background: var(--card);
      border-radius: 15px;
      padding: 20px;
      box-shadow: 0 5px 18px rgba(0, 0, 0, 0.06);
    }

    .sidebar h2 {
      margin-bottom: 15px;
      font-size: 20px;
    }

    .add-section {
      display: flex;
      gap: 8px;
      margin-bottom: 18px;
    }

    input,
    textarea {
      width: 100%;
      border: 1px solid var(--border);
      border-radius: 8px;
      padding: 11px;
      outline: none;
      background: white;
      color: var(--text);
    }

    input:focus,
    textarea:focus {
      border-color: var(--primary);
    }

    .add-section input {
      min-width: 0;
    }

    .primary-btn {
      background: var(--primary);
      color: white;
    }

    .section-list {
      list-style: none;
      display: flex;
      flex-direction: column;
      gap: 8px;
    }

    .section-item {
      display: flex;
      align-items: center;
      justify-content: space-between;
      gap: 5px;
      padding: 10px;
      border-radius: 8px;
      cursor: pointer;
      background: #f9fafb;
      border: 1px solid transparent;
    }

    .section-item:hover,
    .section-item.active {
      background: #eef2ff;
      border-color: var(--primary);
      color: var(--primary-dark);
    }

    .section-name {
      flex: 1;
      overflow: hidden;
      text-overflow: ellipsis;
      white-space: nowrap;
    }

    .delete-btn {
      background: transparent;
      color: var(--danger);
      padding: 4px 7px;
      font-size: 16px;
    }

    .top-content {
      display: flex;
      align-items: center;
      justify-content: space-between;
      gap: 15px;
      flex-wrap: wrap;
      margin-bottom: 20px;
    }

    .top-content h2 {
      font-size: 25px;
    }

    .stats {
      display: flex;
      gap: 10px;
      color: var(--muted);
      font-size: 14px;
    }

    .progress-container {
      margin: 18px 0;
    }

    .progress-bar {
      height: 10px;
      background: #e5e7eb;
      border-radius: 20px;
      overflow: hidden;
      margin-top: 8px;
    }

    .progress-fill {
      height: 100%;
      width: 0;
      background: var(--success);
      transition: width 0.3s;
    }

    .flashcard-area {
      display: flex;
      justify-content: center;
      align-items: center;
      min-height: 350px;
      margin: 20px 0;
      perspective: 1000px;
    }

    .flashcard {
      width: min(100%, 600px);
      height: 310px;
      position: relative;
      cursor: pointer;
      transform-style: preserve-3d;
      transition: transform 0.6s;
    }

    .flashcard.flipped {
      transform: rotateY(180deg);
    }

    .card-face {
      position: absolute;
      inset: 0;
      backface-visibility: hidden;
      border-radius: 18px;
      display: flex;
      justify-content: center;
      align-items: center;
      text-align: center;
      padding: 35px;
      font-size: 25px;
      line-height: 1.6;
      box-shadow: 0 10px 25px rgba(0, 0, 0, 0.12);
      word-break: break-word;
    }

    .front {
      background: linear-gradient(135deg, #6366f1, #8b5cf6);
      color: white;
    }

    .back {
      background: linear-gradient(135deg, #10b981, #059669);
      color: white;
      transform: rotateY(180deg);
    }

    .card-label {
      position: absolute;
      top: 15px;
      font-size: 13px;
      opacity: 0.8;
    }

    [dir="rtl"] .card-label {
      right: 20px;
    }

    [dir="ltr"] .card-label {
      left: 20px;
    }

    .card-actions {
      display: flex;
      justify-content: center;
      gap: 10px;
      flex-wrap: wrap;
    }

    .secondary-btn {
      background: #e5e7eb;
      color: #111827;
    }

    .success-btn {
      background: var(--success);
      color: white;
    }

    .danger-btn {
      background: var(--danger);
      color: white;
    }

    .empty-state {
      text-align: center;
      color: var(--muted);
      padding: 70px 20px;
    }

    .modal {
      display: none;
      position: fixed;
      inset: 0;
      background: rgba(0, 0, 0, 0.5);
      justify-content: center;
      align-items: center;
      padding: 20px;
      z-index: 10;
    }

    .modal.show {
      display: flex;
    }

    .modal-box {
      background: white;
      width: min(100%, 550px);
      padding: 25px;
      border-radius: 15px;
    }

    .modal-box h2 {
      margin-bottom: 18px;
    }

    .form-group {
      margin-bottom: 15px;
    }

    .form-group label {
      display: block;
      margin-bottom: 7px;
      font-weight: bold;
    }

    .modal-actions {
      display: flex;
      justify-content: flex-end;
      gap: 10px;
      margin-top: 20px;
    }

    @media (max-width: 800px) {
      .layout {
        grid-template-columns: 1fr;
      }

      .sidebar {
        order: 1;
      }

      .content-box {
        order: 2;
      }

      .flashcard {
        height: 280px;
      }

      .card-face {
        font-size: 21px;
      }
    }
  </style>
</head>

<body>
  <header>
    <h1 id="appTitle">بطاقاتي التعليمية</h1>
    <button class="language-btn" onclick="toggleLanguage()" id="languageBtn">
      English
    </button>
  </header>

  <main class="layout">
    <aside class="sidebar">
      <h2 id="sectionsTitle">الأقسام</h2>

      <div class="add-section">
        <input id="sectionInput" placeholder="اسم القسم" />
        <button class="primary-btn" onclick="addSection()">+</button>
      </div>

      <ul class="section-list" id="sectionList"></ul>
    </aside>

    <section class="content-box">
      <div id="mainContent"></div>
    </section>
  </main>

  <div class="modal" id="cardModal">
    <div class="modal-box">
      <h2 id="modalTitle">إضافة بطاقة جديدة</h2>

      <div class="form-group">
        <label id="questionLabel" for="questionInput">السؤال</label>
        <textarea id="questionInput" rows="4"></textarea>
      </div>

      <div class="form-group">
        <label id="answerLabel" for="answerInput">الإجابة</label>
        <textarea id="answerInput" rows="4"></textarea>
      </div>

      <div class="modal-actions">
        <button class="secondary-btn" onclick="closeModal()" id="cancelBtn">
          إلغاء
        </button>

        <button class="primary-btn" onclick="saveCard()" id="saveBtn">
          حفظ
        </button>
      </div>
    </div>
  </div>

  <script>
    const STORAGE_KEY = "flash_cards_app_data";

    let language = localStorage.getItem("flash_language") || "ar";
    let data = JSON.parse(localStorage.getItem(STORAGE_KEY)) || {
      sections: [],
      selectedSectionId: null
    };

    let currentCardIndex = 0;
    let editingCardId = null;

    const translations = {
      ar: {
        title: "بطاقاتي التعليمية",
        sections: "الأقسام",
        sectionPlaceholder: "اسم القسم",
        addFirstSection: "أضف قسمًا للبدء",
        noCards: "لا توجد بطاقات في هذا القسم",
        addCard: "إضافة بطاقة",
        question: "السؤال",
        answer: "الإجابة",
        previous: "السابق",
        next: "التالي",
        remembered: "عرفتها",
        forgot: "لم أتذكرها",
        deleteCard: "حذف البطاقة",
        addCardTitle: "إضافة بطاقة جديدة",
        editCardTitle: "تعديل البطاقة",
        cancel: "إلغاء",
        save: "حفظ",
        card: "بطاقة",
        cards: "بطاقات",
        progress: "التقدم",
        clickToFlip: "اضغط على البطاقة لقلبها",
        confirmDeleteSection: "هل تريد حذف هذا القسم وجميع بطاقاته؟",
        confirmDeleteCard: "هل تريد حذف هذه البطاقة؟",
        emptyQuestion: "اكتب السؤال والإجابة أولًا"
      },
      en: {
        title: "My Flash Cards",
        sections: "Sections",
        sectionPlaceholder: "Section name",
        addFirstSection: "Add a section to start",
        noCards: "No cards in this section",
        addCard: "Add Card",
        question: "Question",
        answer: "Answer",
        previous: "Previous",
        next: "Next",
        remembered: "I knew it",
        forgot: "I forgot",
        deleteCard: "Delete Card",
        addCardTitle: "Add New Card",
        editCardTitle: "Edit Card",
        cancel: "Cancel",
        save: "Save",
        card: "Card",
        cards: "Cards",
        progress: "Progress",
        clickToFlip: "Click the card to flip",
        confirmDeleteSection: "Delete this section and all its cards?",
        confirmDeleteCard: "Delete this card?",
        emptyQuestion: "Enter the question and answer first"
      }
    };

    function t(key) {
      return translations[language][key];
    }

    function saveData() {
      localStorage.setItem(STORAGE_KEY, JSON.stringify(data));
    }

    function createId() {
      return Date.now().toString() + Math.random().toString(16).slice(2);
    }

    function toggleLanguage() {
      language = language === "ar" ? "en" : "ar";
      localStorage.setItem("flash_language", language);

      document.documentElement.lang = language;
      document.documentElement.dir = language === "ar" ? "rtl" : "ltr";

      render();
    }

    function addSection() {
      const input = document.getElementById("sectionInput");
      const name = input.value.trim();

      if (!name) return;

      const newSection = {
        id: createId(),
        name,
        cards: []
      };

      data.sections.push(newSection);
      data.selectedSectionId = newSection.id;

      input.value = "";
      saveData();
      render();
    }

    function selectSection(sectionId) {
      data.selectedSectionId = sectionId;
      currentCardIndex = 0;
      saveData();
      render();
    }

    function deleteSection(event, sectionId) {
      event.stopPropagation();

      if (!confirm(t("confirmDeleteSection"))) return;

      data.sections = data.sections.filter(section => section.id !== sectionId);

      if (data.selectedSectionId === sectionId) {
        data.selectedSectionId = data.sections[0]?.id || null;
      }

      currentCardIndex = 0;
      saveData();
      render();
    }

    function getSelectedSection() {
      return data.sections.find(
        section => section.id === data.selectedSectionId
      );
    }

    function openModal(cardId = null) {
      editingCardId = cardId;

      const modal = document.getElementById("cardModal");
      const questionInput = document.getElementById("questionInput");
      const answerInput = document.getElementById("answerInput");

      questionInput.value = "";
      answerInput.value = "";

      if (cardId) {
        const section = getSelectedSection();
        const card = section.cards.find(card => card.id === cardId);

        if (card) {
          questionInput.value = card.question;
          answerInput.value = card.answer;
        }
      }

      document.getElementById("modalTitle").textContent =
        cardId ? t("editCardTitle") : t("addCardTitle");

      modal.classList.add("show");
      questionInput.focus();
    }

    function closeModal() {
      document.getElementById("cardModal").classList.remove("show");
      editingCardId = null;
    }

    function saveCard() {
      const question = document.getElementById("questionInput").value.trim();
      const answer = document.getElementById("answerInput").value.trim();

      if (!question || !answer) {
        alert(t("emptyQuestion"));
        return;
      }

      const section = getSelectedSection();
      if (!section) return;

      if (editingCardId) {
        const card = section.cards.find(card => card.id === editingCardId);

        if (card) {
          card.question = question;
          card.answer = answer;
        }
      } else {
        section.cards.push({
          id: createId(),
          question,
          answer,
          known: false
        });

        currentCardIndex = section.cards.length - 1;
      }

      saveData();
      closeModal();
      render();
    }

    function deleteCard(cardId) {
      if (!confirm(t("confirmDeleteCard"))) return;

      const section = getSelectedSection();
      section.cards = section.cards.filter(card => card.id !== cardId);

      if (currentCardIndex >= section.cards.length) {
        currentCardIndex = Math.max(section.cards.length - 1, 0);
      }

      saveData();
      render();
    }

    function flipCard() {
      const card = document.querySelector(".flashcard");
      if (card) card.classList.toggle("flipped");
    }

    function changeCard(direction) {
      const section = getSelectedSection();
      if (!section || section.cards.length === 0) return;

      currentCardIndex += direction;

      if (currentCardIndex < 0) {
        currentCardIndex = section.cards.length - 1;
      }

      if (currentCardIndex >= section.cards.length) {
        currentCardIndex = 0;
      }

      render();
    }

    function markCard(known) {
      const section = getSelectedSection();
      if (!section || !section.cards[currentCardIndex]) return;

      section.cards[currentCardIndex].known = known;
      saveData();

      if (known) {
        changeCard(1);
      } else {
        render();
      }
    }

    function renderSections() {
      const sectionList = document.getElementById("sectionList");

      if (data.sections.length === 0) {
        sectionList.innerHTML = `
          <li style="color:#6b7280; text-align:center;">
            ${t("addFirstSection")}
          </li>
        `;
        return;
      }

      sectionList.innerHTML = data.sections.map(section => `
        <li
          class="section-item ${section.id === data.selectedSectionId ? "active" : ""}"
          onclick="selectSection('${section.id}')"
        >
          <span class="section-name">
            ${escapeHTML(section.name)}
          </span>

          <button
            class="delete-btn"
            onclick="deleteSection(event, '${section.id}')"
            title="Delete"
          >
            ×
          </button>
        </li>
      `).join("");
    }

    function renderMainContent() {
      const mainContent = document.getElementById("mainContent");
      const section = getSelectedSection();

      if (!section) {
        mainContent.innerHTML = `
          <div class="empty-state">
            <h2>${t("addFirstSection")}</h2>
          </div>
        `;
        return;
      }

      const cards = section.cards;

      if (cards.length === 0) {
        mainContent.innerHTML = `
          <div class="top-content">
            <h2>${escapeHTML(section.name)}</h2>
            <button class="primary-btn" onclick="openModal()">
              + ${t("addCard")}
            </button>
          </div>

          <div class="empty-state">
            <p>${t("noCards")}</p>
          </div>
        `;
        return;
      }

      if (currentCardIndex >= cards.length) {
        currentCardIndex = cards.length - 1;
      }

      const card = cards[currentCardIndex];
      const knownCards = cards.filter(card => card.known).length;
      const progress = Math.round((knownCards / cards.length) * 100);

      mainContent.innerHTML = `
        <div class="top-content">
          <div>
            <h2>${escapeHTML(section.name)}</h2>
            <div class="stats">
              <span>${t("card")} ${currentCardIndex + 1} / ${cards.length}</span>
              <span>${t("progress")}: ${progress}%</span>
            </div>
          </div>

          <button class="primary-btn" onclick="openModal()">
            + ${t("addCard")}
          </button>
        </div>

        <div class="progress-container">
          <div class="progress-bar">
            <div class="progress-fill" style="width:${progress}%"></div>
          </div>
        </div>

        <div class="flashcard-area">
          <div class="flashcard" onclick="flipCard()">
            <div class="card-face front">
              <span class="card-label">${t("question")}</span>
              ${escapeHTML(card.question)}
            </div>

            <div class="card-face back">
              <span class="card-label">${t("answer")}</span>
              ${escapeHTML(card.answer)}
            </div>
          </div>
        </div>

        <p style="text-align:center;color:#6b7280;margin-bottom:15px;">
          ${t("clickToFlip")}
        </p>

        <div class="card-actions">
          <button class="secondary-btn" onclick="changeCard(-1)">
            ${t("previous")}
          </button>

          <button class="success-btn" onclick="markCard(true)">
            ${t("remembered")}
          </button>

          <button class="danger-btn" onclick="markCard(false)">
            ${t("forgot")}
          </button>

          <button class="secondary-btn" onclick="changeCard(1)">
            ${t("next")}
          </button>

          <button class="secondary-btn" onclick="openModal('${card.id}')">
            ${language === "ar" ? "تعديل" : "Edit"}
          </button>

          <button class="danger-btn" onclick="deleteCard('${card.id}')">
            ${t("deleteCard")}
          </button>
        </div>
      `;
    }

    function renderTexts() {
      document.getElementById("appTitle").textContent = t("title");
      document.getElementById("sectionsTitle").textContent = t("sections");
      document.getElementById("sectionInput").placeholder = t("sectionPlaceholder");
      document.getElementById("languageBtn").textContent =
        language === "ar" ? "English" : "العربية";

      document.getElementById("questionLabel").textContent = t("question");
      document.getElementById("answerLabel").textContent = t("answer");
      document.getElementById("cancelBtn").textContent = t("cancel");
      document.getElementById("saveBtn").textContent = t("save");
    }

    function render() {
      document.documentElement.lang = language;
      document.documentElement.dir = language === "ar" ? "rtl" : "ltr";

      renderTexts();
      renderSections();
      renderMainContent();
    }

    function escapeHTML(text) {
      return text
        .replace(/&/g, "&amp;")
        .replace(/</g, "&lt;")
        .replace(/>/g, "&gt;")
        .replace(/"/g, "&quot;")
        .replace(/'/g, "&#039;");
    }

    document.getElementById("sectionInput").addEventListener("keydown", event => {
      if (event.key === "Enter") {
        addSection();
      }
    });

    document.getElementById("cardModal").addEventListener("click", event => {
      if (event.target.id === "cardModal") {
        closeModal();
      }
    });

    render();
  </script>
</body>
</html>
