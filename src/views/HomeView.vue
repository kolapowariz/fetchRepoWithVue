<script setup>
import router from '@/router';
import Hello from '/Hello-rafiki.png'
import { useRoute } from 'vue-router';
const route = useRoute()

const goToRepoList = () => {
  router.push('repo')
}
</script>

<template>
  <main class="text-center mt-4">
    
    <h2 className="text-3xl mt-10 sm:text-4xl">Fetch My Repo with Vue</h2>
    <div class="md:w-2/5 sm:mt-0 mx-auto">
      <img :src='Hello' alt="Hello" >
    </div>
    <button @click="goToRepoList" class="h-10 w-28 rounded-md bg-blue-600 px-4 text-xl font-semibold leading-6 text-white shadow-sm hover:bg-blue-400 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-blue-600">Continue</button>
    

  </main>
</template>
