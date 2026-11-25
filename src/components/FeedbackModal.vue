<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const emit = defineEmits(['close', 'submit'])

const rating = ref(0)
const comment = ref('')
const showError = ref(false)

function setRating(value) {
  rating.value = value
  showError.value = false
}

function submit() {
  if (!rating.value) {
    showError.value = true
    return
  }
  emit('submit', {
    rating: rating.value,
    comment: comment.value.trim(),
  })
}

function onKeyDown(event) {
  if (event.key === 'Escape') emit('close')
}

onMounted(() => window.addEventListener('keydown', onKeyDown))
onUnmounted(() => window.removeEventListener('keydown', onKeyDown))
</script>

<template>
  <div class="overlay" @click.self="emit('close')">
    <div class="modal" role="dialog" aria-modal="true">
      <button
        class="modal-close"
        type="button"
        aria-label="Закрыть"
        @click="emit('close')"
      >×</button>

      <h2 class="modal-title">Обратная связь</h2>

      <div class="field">
        <span class="label">Оценка мероприятия</span>
        <div class="stars">
          <button
            v-for="star in 5"
            :key="star"
            type="button"
            class="star"
            :class="{ active: star <= rating }"
            @click="setRating(star)"
          >★</button>
        </div>
        <p v-if="showError" class="error">Выберите оценку</p>
      </div>

      <div class="field">
        <label class="label" for="comment">Комментарий</label>
        <textarea
          id="comment"
          v-model="comment"
          class="comment"
          rows="4"
          maxlength="500"
          placeholder="Ваш комментарий (необязательно)"
        ></textarea>
        <p class="counter">{{ comment.length }} / 500</p>
      </div>

      <button class="submit" type="button" @click="submit">Отправить отзыв</button>
    </div>
  </div>
</template>

<style scoped>
.overlay {
  position: fixed;
  inset: 0;
  z-index: 900;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 20px;
  background: rgba(0, 0, 0, 0.45);
  overflow-y: auto;
}

.modal {
  position: relative;
  width: 100%;
  max-width: 440px;
  max-height: calc(100vh - 40px);
  padding: 20px;
  background: #ffffff;
  border: 1px solid #d9dee5;
  border-radius: 8px;
  overflow-y: auto;
  font-family: Arial, Helvetica, sans-serif;
  color: #243044;
}

.modal-close {
  position: absolute;
  top: 10px;
  right: 10px;
  width: 32px;
  height: 32px;
  font-size: 22px;
  line-height: 1;
  color: #8a94a3;
  background: none;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.modal-close:hover {
  color: #d3453b;
  background: #fdeeed;
}

.modal-title {
  margin: 0 0 16px;
  font-size: 20px;
}

.field {
  margin-bottom: 16px;
}

.label {
  display: block;
  margin-bottom: 6px;
  font-size: 14px;
  color: #55606e;
}

.stars {
  display: flex;
  gap: 4px;
}

.star {
  padding: 4px;
  font-size: 30px;
  line-height: 1;
  color: #c3cbd6;
  background: none;
  border: none;
  cursor: pointer;
}

.star.active {
  color: #f5a623;
}

.error {
  margin: 6px 0 0;
  font-size: 13px;
  color: #d3453b;
}

.comment {
  width: 100%;
  box-sizing: border-box;
  padding: 8px 10px;
  font-size: 15px;
  font-family: Arial, Helvetica, sans-serif;
  color: #243044;
  border: 1px solid #c3cbd6;
  border-radius: 6px;
  resize: vertical;
}

.counter {
  margin: 4px 0 0;
  font-size: 12px;
  color: #98a1ad;
  text-align: right;
}

.submit {
  width: 100%;
  padding: 12px;
  font-size: 15px;
  font-family: Arial, Helvetica, sans-serif;
  color: #ffffff;
  background: #2f6fde;
  border: 1px solid #2f6fde;
  border-radius: 6px;
  cursor: pointer;
}

.submit:hover {
  background: #275fc0;
}

@media (max-width: 480px) {
  .overlay {
    padding: 12px;
  }

  .modal {
    max-width: none;
    min-height: 85vh;
    max-height: calc(100vh - 24px);
  }

  .star {
    font-size: 36px;
    padding: 6px;
  }

  .comment {
    font-size: 16px;
    padding: 10px 12px;
  }

  .submit {
    padding: 14px;
    font-size: 16px;
  }

  .modal-close {
    width: 40px;
    height: 40px;
    font-size: 26px;
  }
}
</style>
