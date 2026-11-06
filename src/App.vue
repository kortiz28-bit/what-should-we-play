<script setup>
import { ref } from 'vue'

const currentPage = ref('home')
const games = ref(['Fortnite', 'Minecraft', 'Roblox', 'Deadlock'])
const newGame = ref('')
const selectedGame = ref('')

function pickGame() {
  if (games.value.length === 0) return

  const randomIndex = Math.floor(Math.random() * games.value.length)
  selectedGame.value = games.value[randomIndex]
}

function addGame() {
  const name = newGame.value.trim()

  if (name !== '') {
    games.value.push(name)
    newGame.value = ''
  }
}

function removeGame(index) {
  games.value.splice(index, 1)
  selectedGame.value = ''
}
</script>

<template>
  <div class="min-h-screen bg-blue-50 px-5 py-8 text-blue-950">
    <nav class="mb-10 flex justify-center gap-4">
      <button
        @click="currentPage = 'home'"
        :class="currentPage === 'home' ? 'bg-blue-400 text-white' : 'bg-transparent text-blue-950'"
        class="cursor-pointer rounded-lg px-6 py-3 hover:bg-blue-300"
      >
        Home
      </button>

      <button
        @click="currentPage = 'library'"
        :class="currentPage === 'library' ? 'bg-blue-400 text-white' : 'bg-transparent text-blue-950'"
        class="cursor-pointer rounded-lg px-6 py-3 hover:bg-blue-300"
      >
        Library
      </button>
    </nav>

    <main class="flex justify-center">
      <section
        v-if="currentPage === 'home'"
        class="w-full max-w-xl rounded-2xl bg-white p-10 text-center shadow-md"
      >
        <h1 class="mb-4 text-3xl font-bold">Random Game Picker</h1>
        <p class="mb-8">Ready to find your next game?</p>

        <button
          @click="pickGame"
          :disabled="games.length === 0"
          class="cursor-pointer rounded-lg bg-blue-400 px-7 py-3 text-white hover:bg-blue-500 disabled:cursor-not-allowed disabled:opacity-50"
        >
          Pick a Game
        </button>

        <p v-if="selectedGame" class="mt-6 text-xl font-bold">
          {{ selectedGame }}
        </p>
      </section>

      <section
        v-if="currentPage === 'library'"
        class="w-full max-w-xl rounded-2xl bg-white p-10 text-center shadow-md"
      >
        <h1 class="mb-4 text-3xl font-bold">Game Library</h1>
        <p>Add or remove games from your library.</p>

        <form @submit.prevent="addGame" class="my-8 flex flex-col gap-3 sm:flex-row">
          <input
            v-model="newGame"
            type="text"
            placeholder="Enter game name"
            class="min-w-0 flex-1 rounded-lg border border-slate-300 px-4 py-3"
          />

          <button
            type="submit"
            class="cursor-pointer rounded-lg bg-blue-400 px-6 py-3 text-white hover:bg-blue-500"
          >
            Add Game
          </button>
        </form>

        <div class="flex flex-col gap-3">
          <div
            v-for="(game, index) in games"
            :key="index"
            class="flex items-center justify-between gap-3 rounded-lg bg-blue-50 p-3 text-left"
          >
            <span>{{ game }}</span>

            <button
              @click="removeGame(index)"
              class="cursor-pointer rounded-lg bg-rose-400 px-4 py-2 text-white hover:bg-rose-500"
            >
              Remove
            </button>
          </div>
        </div>

        <p v-if="games.length === 0" class="mt-5">
          Your library is empty.
        </p>
      </section>
    </main>
  </div>
</template>
