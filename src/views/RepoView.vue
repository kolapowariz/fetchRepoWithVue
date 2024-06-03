<script setup>
import Hello from '/Hello-rafiki.png'
import { ref, onMounted, computed} from 'vue'
const fetching = ref(null)
const error = ref(null);
const isLoading = ref(false)
const currentPage = ref(1)
const lastPage = ref(null)
const perPage = ref(10)
import router from '@/router'
import { useRoute } from 'vue-router';




onMounted(() => {
  repoFetch()
})

const repoFetch = () => {
  isLoading.value = true
  fetch(`https://api.github.com/users/kolapowariz/repos?per_page=${perPage.value}&page=${currentPage.value}`)
    .then((res) => {
      if (!res.ok) {
        throw new Error('Response was not good')
      }
      const linkHeader = res.headers.get('Link');
      if (linkHeader) {
        const lastLink = linkHeader.split(',').find(link => link.includes('rel="last"'));
        if (lastLink) {
          const match = lastLink.match(/&page=(\d+)/)
          if (match) {
            lastPage.value = Number(match[1])
          }
        }
      }
      return res.json()
    })
    .then(data => {
      fetching.value = data
    })
    .catch(err => {
      error.value = err.message;
    })
    .finally(() => {
      isLoading.value = false
    })
}


const next = () => {
  if (currentPage.value < lastPage.value) {
    currentPage.value++
    repoFetch()
  }
}

const prev = () => {
  if (currentPage.value > 1) {
    currentPage.value--
    repoFetch()
  }
}

const search = ref('');
const filteredRepos = computed(() => {
    if (!search.value || !fetching.value) {
      return fetching.value;
    }
    return fetching.value.filter(repo => repo.name.toLowerCase().includes(search.value.toLowerCase()))
})
// const route = useRoute()

const test404 = () => {
  router.push('/remit')
}


</script>

<template>
  <main class="text-center mt-4">
    <h2 className="text-3xl my-10 sm:text-4xl">Fetch My Repo with Vue</h2>
    <div v-if="isLoading" class="text-2xl mt-4">
      <div>Loading...</div>
      
    </div>
    <div v-else class="flex flex-col justify-center items-center">
      <input type="text" v-model="search" class="mb-10 h-10 text-center w-80 rounded-md border-0 py-1.5 px-2 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 placeholder:text-center focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-xl sm:leading-6 sm:placeholder:text-xl" placeholder="Search for repo">
      <ul class="mb-6 flex justify-center flex-col items-center gap-6 sm:grid sm:grid-cols-2 ">
        <li v-for="(fetch, index) in filteredRepos" :key="index" class="border h-16 rounded-md shadow w-80 sm:w-72">
          <router-link :to="`/detailed/${fetch.name}`" class="text-xl">{{ fetch.name }}</router-link>
        </li>
      </ul>
      <div class="mb-6 w-80  flex items-center justify-around">
        <button @click="prev" :disabled="currentPage === 1" class="h-10 w-28 rounded-md bg-blue-600 px-4 py-auto text-xl font-semibold leading-6 text-white shadow-sm disabled:cursor-not-allowed disabled:bg-blue-300 hover:bg-blue-500 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-blue-600">Previous</button>
        <button @click="next" :disabled="currentPage === lastPage" class="h-10 w-28 rounded-md bg-blue-600 px-4 py-auto text-xl font-semibold leading-6 text-white shadow-sm disabled:cursor-not-allowed disabled:bg-blue-300 hover:bg-blue-500 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-blue-600">Next</button>        
      </div>
      <div>
        <button @click="test404" class="mb-20 h-10 w-28 rounded-md bg-blue-600 px-4 py-auto text-xl font-semibold leading-6 text-white shadow-sm hover:bg-blue-500 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-blue-600">Test 404</button>
    </div>
    </div>
  </main>
</template>
