<script setup>
import { ref, onMounted } from 'vue'
import { useRoute } from 'vue-router';

const repo = ref(null)
const error = ref(null)
const isLoading = ref(true)

const loadRepo = async () => {
  const { params } = useRoute()
  try {
    const response = await fetch(`https://api.github.com/repos/kolapowariz/${params.repoName}`)
    if (!response.ok) {
      throw new Error('Failed to load repo')
    }
    repo.value = await response.json();
  } catch (err) {
    error.value = err.message
  } finally {
    isLoading.value = false
  }
}

onMounted(loadRepo)

</script>
<template>
  <section>
    <section v-if="isLoading" class="text-center text-2xl mt-4">Loading...</section>
    <section v-else class=" mt-4 mx-auto p-4 rounded-xl text-center gap-3 sm:grid sm:grid-cols-2 max-w-3xl">
      <h2 class="border rounded-md shadow-md my-4 text-2xl py-4">Repo Name
         <p class="text-xl mt-2">
          {{ repo.name }}
        </p>
      </h2>
      <p class="border rounded-md shadow-md my-4 p-4 text-2xl">Repo ID
      <p class="text-xl mt-2">{{ repo.id }}</p>
      </p>
      <p class="border rounded-md shadow-md my-4 p-4 text-2xl">Repo Archieve URL
      <p class="text-xl mt-2"> {{ repo.archive_url }}</p>
      </p>
      <p class="border rounded-md shadow-md my-4 p-4 text-2xl">Repo Allow Forking
      <p class="text-xl mt-2">{{ repo.allow_forking }}</p>
      </p>
      <p class="border rounded-md shadow-md my-4 p-4 text-2xl">Repo Archieved
      <p class="text-xl mt-2">{{ repo.archived }}</p>
      </p>
      <p class="border rounded-md shadow-md my-4 p-4 text-2xl">Repo Blobs URL
      <p class="text-xl mt-2">{{ repo.blobs_url }}</p>
      </p>
      <p class="border rounded-md shadow-md my-4 p-4 text-2xl">Repo Assignees URL
      <p class="text-xl mt-2">{{ repo.assignees_url }}</p>
      </p>
      <p class="border rounded-md shadow-md my-4 p-4 text-2xl">Repo Branches URL
      <p class="text-xl mt-2">{{ repo.branches_url }}</p>
      </p>
      <p class="border rounded-md shadow-md my-4 p-4 text-2xl">Repo Clone URL
      <p class="text-xl mt-2">{{ repo.clone_url }}</p>
      </p>
      <p class="border rounded-md shadow-md my-4 p-4 text-2xl">Repo Forked
      <p class="text-xl mt-2">{{ repo.fork }}</p>
      </p>
      <p class="border rounded-md shadow-md my-4 p-4 text-2xl">Repo Homepage
      <p class="text-xl mt-2">{{ repo.homepage }}</p>
      </p>
      <p class="border rounded-md shadow-md my-4 p-4 text-2xl">Repo Language
      <p class="text-xl mt-2">{{ repo.language }}</p>
      </p>
    </section>
  </section>
</template>