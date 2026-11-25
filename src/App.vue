<script setup>
import { ref } from 'vue'
import FeedbackModal from './components/FeedbackModal.vue'
import ToastNotification from './components/ToastNotification.vue'

const STORAGE_KEY = 'feedback-list'

function loadFeedbackList() {
  try {
    return JSON.parse(localStorage.getItem(STORAGE_KEY)) || []
  } catch {
    return []
  }
}

const showModal = ref(false)
const showToast = ref(false)
const feedbackSent = ref(localStorage.getItem(STORAGE_KEY) !== null)

function openModal() {
  showModal.value = true
}

function closeModal() {
  showModal.value = false
}

function submitFeedback(review) {
  const list = loadFeedbackList()
  list.push({
    rating: review.rating,
    comment: review.comment,
    date: new Date().toLocaleDateString('ru-RU'),
  })
  localStorage.setItem(STORAGE_KEY, JSON.stringify(list))
  showModal.value = false
  feedbackSent.value = true
  showToast.value = true
}
</script>

<template>
  <div class="app">
    <header class="site-header">Конференции.РФ</header>

    <main class="page">
      <section class="card">
        <h1 class="event-title">Конференция «Цифровая экономика 2026»</h1>
        <p class="event-info">Дата проведения: 12.11.2026</p>
        <p class="event-info status">Статус: Завершено</p>
      </section>

      <section class="card">
        <h2 class="section-title">Заявки на участие</h2>
        <table class="requests">
          <thead>
            <tr>
              <th>№</th>
              <th>ФИО участника</th>
              <th>Дата подачи</th>
              <th>Статус</th>
            </tr>
          </thead>
          <tbody>
            <tr>
              <td>1</td>
              <td>Иванов Иван Иванович</td>
              <td>01.11.2026</td>
              <td>Одобрена</td>
            </tr>
            <tr>
              <td>2</td>
              <td>Петров Пётр Петрович</td>
              <td>03.11.2026</td>
              <td>Одобрена</td>
            </tr>
            <tr>
              <td>3</td>
              <td>Сидорова Анна Александровна</td>
              <td>05.11.2026</td>
              <td>Одобрена</td>
            </tr>
          </tbody>
        </table>
      </section>

      <section class="card feedback">
        <button v-if="!feedbackSent" class="btn" @click="openModal">
          Оставить отзыв
        </button>
        <p v-else class="sent-text">Ваш отзыв отправлен</p>
      </section>
    </main>

    <FeedbackModal v-if="showModal" @close="closeModal" @submit="submitFeedback" />
    <ToastNotification v-if="showToast" @hide="showToast = false" />
  </div>
</template>

<style scoped>
.app {
  min-height: 100vh;
  font-family: Arial, Helvetica, sans-serif;
  color: #243044;
  background: #f2f4f7;
}

.site-header {
  padding: 14px 20px;
  font-size: 20px;
  font-weight: bold;
  color: #ffffff;
  background: #2f6fde;
}

.page {
  max-width: 760px;
  margin: 0 auto;
  padding: 24px 16px;
}

.card {
  background: #ffffff;
  border: 1px solid #d9dee5;
  border-radius: 8px;
  padding: 20px;
  margin-bottom: 16px;
}

.event-title {
  margin: 0 0 12px;
  font-size: 22px;
}

.event-info {
  margin: 4px 0;
  font-size: 15px;
  color: #55606e;
}

.event-info.status {
  color: #2e7d32;
}

.section-title {
  margin: 0 0 12px;
  font-size: 18px;
}

.requests {
  width: 100%;
  border-collapse: collapse;
  font-size: 14px;
}

.requests th,
.requests td {
  padding: 8px 10px;
  border: 1px solid #e2e6ec;
  text-align: left;
}

.requests th {
  background: #eef1f5;
}

.feedback {
  text-align: center;
}

.btn {
  padding: 12px 24px;
  font-size: 16px;
  font-family: Arial, Helvetica, sans-serif;
  color: #ffffff;
  background: #2f6fde;
  border: 1px solid #2f6fde;
  border-radius: 6px;
  cursor: pointer;
}

.btn:hover {
  background: #275fc0;
}

.sent-text {
  margin: 0;
  font-size: 16px;
  color: #2e7d32;
}

@media (max-width: 480px) {
  .page {
    padding: 12px;
  }

  .card {
    padding: 14px;
  }

  .requests th,
  .requests td {
    padding: 10px 8px;
    font-size: 13px;
  }

  .btn {
    padding: 14px 28px;
    font-size: 17px;
  }
}
</style>
