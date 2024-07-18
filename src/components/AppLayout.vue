<script setup lang="ts">
import { ref, computed, onMounted } from 'vue'

const text = ref('')
const links = ref([])
const input_type = ref('link')

const search = computed<any>(() => {
  if (text.value == '' || input_type.value != 'link') return links.value
  return links.value.filter((item: any) => item.key.includes(text.value))
})

function openLink(link: string) {
  window.open(link, '_blank')
}

function handleKeydown(event: KeyboardEvent) {
  if (event.key == 'Enter' && input_type.value == 'link') {
    openLink(search.value[0].link)
  } else if (event.key == 'Enter' && input_type.value == 'search') {
    openLink('https://www.google.com/search?q=' + text.value)
  } else if (event.key == 'Escape') {
    text.value = ''
  } else if (event.altKey && event.key == 'g') {
    input_type.value = 'search'
  } else if (event.altKey && event.key == 'l') {
    input_type.value = 'link'
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
          <q-input 
          autofocus outlined 
          placeholder="谷歌搜索: Alt+G  搜索导航: Alt+L"
          v-model="text" @keydown="handleKeydown">
            <template v-slot:prepend>
              <q-icon :name="input_type" />
            </template>
          </q-input>
          <div class="row justify-center q-mt-sm q-gutter-sm">
            <q-btn outline color="primary" @click="openLink(link)" :label="name" v-for="{ key, name, link } in search"
              :key="key" />
          </div>
        </div>
        <div class="col"></div>
      </div>
    </q-page-container>
  </q-layout>
</template>
