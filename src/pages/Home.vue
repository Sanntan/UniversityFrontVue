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
        class="shadow-xl border border-slate-0 bg-slate-100 resize-none outline-none rounded-3xl px-3 py-2 placeholder:italic bg-gradient-to-br from-sky-200 via-sky-100 to-pink-200 mt-20 text-black font-bold bg-opacity-50"
        rows="4"
        cols="30"
        placeholder="Введите текст"
        v-model="text"
      ></textarea>

      <input
        class="cursor-pointer bg-white-600 rounded-3xl text-black py-2 border-2 border-black"
        type="submit"
        value="Отправить"
      />
    </form>

    <form class="flex flex-col w-fit mx-auto gap-4" @submit.prevent="onSummarizationClick">
      <textarea
        class="shadow-xl border border-slate-0 bg-slate-100 resize-none outline-none rounded-3xl px-3 py-2 placeholder:italic bg-gradient-to-br from-sky-200 via-sky-100 to-pink-200 mt-4 text-black font-bold bg-opacity-50"
        rows="6"
        cols="100"
        placeholder="Обработанный текст"
        v-model="text"
      ></textarea>

      <div class="flex flex-col gap-2 mt-4">
        <div class="flex justify-between items-center">
          <select v-model="summarizationMethod">
            <option value="abstractive">Абстрактивная</option>
            <option value="extractive">Экстрактивная</option>
          </select>

          <div class="flex items-center gap-4" v-if="summarizationMethod === 'extractive'">
            <p>Кол-во предложений:</p>
            <input
              class="border border-slate-300 bg-slate-100 rounded-md px-2 py-2 w-24 text-center rounded-2xl"
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