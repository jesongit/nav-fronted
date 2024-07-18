<script setup lang="ts">
import { ref, computed} from 'vue'

const text = ref('')
const links = ref([])

const search = computed<any>(() => {
  if (text.value == '') return links.value
  return links.value.filter((item: any) => item.key.includes(text.value))
})

function openLink(link: string) {
  window.open(link, '_blank')
}

function handleKeydown(event: KeyboardEvent) {
  if (event.key === 'Enter') {
    openLink(search.value[0].link)
  } else if (event.key === 'Escape') {
    text.value = ''
  }
}

fetch('links.json')
  .then(response => response.json())
  .then(data => {
    links.value = data;
  })
  .catch(error => {
    console.log(error);
  });
</script>

<template>
  <q-layout view="hHh LpR fff">
    <q-page-container>
      <div class="row q-mt-md">
        <div class="col"></div>
        <div class="col-6">
          <q-input autofocus outlined v-model="text" @keydown="handleKeydown" />
          <div class="row justify-center q-mt-sm q-gutter-sm">
            <q-btn
              outline
              color="primary"
              @click="openLink(link)"
              :label="name"
              v-for="{ key, name, link } in search"
              :key="key"
            />
          </div>
        </div>
        <div class="col"></div>
      </div>
    </q-page-container>
  </q-layout>
</template>
