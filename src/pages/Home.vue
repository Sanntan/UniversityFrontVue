<script setup>
import { ref } from 'vue'
import {
  extractiveSummarization,
  abstractiveSummarization
} from '../services/summarizationService.js'

const text = ref('')
const numberSentences = ref(3)
const summarizationMethod = ref('extractive')

async function onSummarizationClick() {
  if (summarizationMethod === 'extractive') {
    text.value = await extractiveSummarization(text.value, numberSentences.value)
  } else {
    text.value = await abstractiveSummarization(text.value)
  }
}
</script>

<template>
  <div class="mt-16">
    <form class="flex flex-col w-fit mx-auto gap-3" @submit.prevent="onSummarizationClick">
      <textarea
        class="shadow-xl resize-none outline-none rounded-3xl px-3 py-2 bg-gray-100 border mt-20 placeholder-gray-900 text-gray-900 font-sans placeholder:italic 
        dark:bg-gray-800 dark:text-gray-300 dark:placeholder-gray-300"
        rows="6"
        cols="30"
        placeholder="Вставьте файл"
        v-model="text"
      ></textarea>

      <input
        class="mt-5 mb-7 cursor-pointer bg-white-600 rounded-3xl text-black py-2 border-2 border-black"
        type="submit"
        value="Отправить"
      />
    </form>

    <form class="flex flex-col w-fit mx-auto gap-4" @submit.prevent="onSummarizationClick">
      <textarea
        class="shadow-xl resize-none outline-none rounded-3xl px-3 py-2 bg-gray-100 border placeholder-gray-900 text-gray-900 font-sans placeholder:italic"
        rows="8"
        cols="100"
        placeholder="Обработанный текст"
        v-model="text"
      ></textarea>

      <div class="flex flex-col text-lg font-sans">
        <div class="flex justify-between items-center">
          <select v-model="summarizationMethod">
            <option value="abstractive">Абстрактивная</option>
            <option value="extractive">Экстрактивная</option>
          </select>

          <div class="flex items-center gap-4" v-if="summarizationMethod === 'extractive'">
            <p>Кол-во предложений:</p>
            <input
              class="border border-slate-300 bg-slate-100 px-2 py-2 w-24 text-center rounded-xl shadow-xl"
              type="number"
              value="3"
              min="3"
              v-model="numberSentences"
            />
          </div>
        </div>
      </div>
    </form>
  </div>
</template>