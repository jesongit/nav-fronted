<script setup lang="ts">
import { ref, computed, onBeforeMount } from 'vue'

const text = ref('')
const links = ref([{ key: 'bk', name: '百度', link: 'https://www.baidu.com' }])
const links_url = 'https://raw.githubusercontent.com/jesongit/nav/master/config.json'

const search = computed(() => {
  if (text.value == '') return links.value
  return links.value.filter((item) => item.key.includes(text.value))
})

function openLink(link: string) {
  window.open(link, '_blank')
}

async function getRemoteJson(url: string): Promise<any> {
  try {
    const response = await fetch(url)
    const data = await response.json()
    return data
  } catch (error) {
    console.error('获取远程JSON文件失败:', error)
  }
}

function refresh() {
  getRemoteJson(links_url).then((data) => {
    console.log(data)
    links.value = data.links
  })
}

function handleKeydown(event: KeyboardEvent) {
  if (event.key === 'Enter') {
    openLink(search.value[0].link)
  } else if (event.key === 'Escape') {
    text.value = ''
  }
}

onBeforeMount(() => {
  refresh()
})
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
