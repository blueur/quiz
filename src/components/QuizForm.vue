<script setup lang="ts">
import QuestionRadio from '@/components/QuestionRadio.vue'
import QuestionText from '@/components/QuestionText.vue'
import { computed, ref } from 'vue'

const cheval = ref<string | null>(null)
const chat = ref<string | null>(null)
const filled = computed<boolean>(
  () => cheval.value !== null && chat.value !== null,
)

function submit(event: Event): void {
  event.preventDefault()
  let score: number = 0
  if (cheval.value === 'blanc') {
    score += 1
  }
  if (chat.value === '4') {
    score += 1
  }
  alert(`Votre score est de ${score} sur 2`)
}

function reset(event: Event): void {
  event.preventDefault()
  cheval.value = null
  chat.value = null
}
</script>

<template>
  <form>
    <QuestionRadio
      id="cheval"
      v-model="cheval"
      text="De quelle couleur est le cheval blanc de Napoléon ?"
      :options="[
        { value: 'blanc', text: 'Blanc' },
        { value: 'brun', text: 'Brun' },
        { value: 'noir', text: 'Noir' },
      ]"
    />
    <QuestionText
      id="chat"
      v-model="chat"
      text="Combien de pattes a un chat ?"
      placeholder="Veuillez saisir un nombre"
    />
    <br />
    <button
      class="btn btn-primary"
      :class="{ disabled: !filled }"
      @click="submit"
    >
      Terminer
    </button>
    <button class="btn btn-secondary" @click="reset">Réinitialiser</button>
  </form>
</template>
