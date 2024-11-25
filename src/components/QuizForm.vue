<script setup lang="ts">
import QuestionRadio from '@/components/QuestionRadio.vue'
import QuestionText from '@/components/QuestionText.vue'
import { QuestionState } from '@/utils/models'
import { computed, ref } from 'vue'

const questionStates = ref<QuestionState[]>([])
const filled = computed<boolean>(() =>
  questionStates.value.every(state => state === QuestionState.Fill),
)
const submitted = computed<boolean>(() =>
  questionStates.value.every(
    state => state === QuestionState.Correct || state === QuestionState.Wrong,
  ),
)
const score = computed<number>(
  () =>
    questionStates.value.filter(state => state === QuestionState.Correct)
      .length,
)
const totalScore = computed<number>(() => questionStates.value.length)

function submit(event: Event): void {
  event.preventDefault()
  questionStates.value = questionStates.value.map(() => QuestionState.Submit)
}

function reset(event: Event): void {
  event.preventDefault()
  questionStates.value = questionStates.value.map(() => QuestionState.Empty)
}
</script>

<template>
  <form>
    <QuestionRadio
      id="cheval"
      v-model="questionStates[0]"
      text="De quelle couleur est le cheval blanc de Napoléon ?"
      :options="[
        { value: 'blanc', text: 'Blanc' },
        { value: 'brun', text: 'Brun' },
        { value: 'noir', text: 'Noir' },
      ]"
      answer="blanc"
    />
    <QuestionText
      id="chat"
      v-model="questionStates[1]"
      text="Combien de pattes a un chat ?"
      answer="4"
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
    &nbsp;
    <button class="btn btn-secondary" @click="reset">Réinitialiser</button>
    <div v-if="submitted">Score : {{ score }} / {{ totalScore }}</div>
    <div>Debug états : {{ questionStates }}</div>
  </form>
</template>
