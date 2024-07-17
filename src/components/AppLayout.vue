<script setup lang="ts">
import { ref, computed } from 'vue'

const text = ref('')

const links = ref([
    { key: 'bd1', name: '百度', link: 'https://www.baidu.com' },
    { key: 'bd2', name: '百度', link: 'https://www.baidu.com' },
    { key: 'bd3', name: '百度', link: 'https://www.baidu.com' },
    { key: 'bd4', name: '百度', link: 'https://www.baidu.com' },
    { key: 'bd5', name: '百度', link: 'https://www.baidu.com' },
    { key: 'bd6', name: '百度', link: 'https://www.baidu.com' },
    { key: 'bd7', name: '百度', link: 'https://www.baidu.com' },
    { key: 'bd8', name: '百度', link: 'https://www.baidu.com' },
    { key: 'bd9', name: '百度', link: 'https://www.baidu.com' },
    { key: 'bd10', name: '百度', link: 'https://www.baidu.com' },
])

const search = computed(() => {
    if(text.value == '')
        return links.value
    return links.value.filter((item) => item.key.includes(text.value))
})

function openLink(link: string) {
    window.open(link, '_blank')
}

</script>


<template>
    <q-layout view="hHh LpR fff">

        <!-- <q-header elevated class="bg-primary text-white">
      <q-toolbar>
        <q-toolbar-title>
          <q-avatar>
            <img src="https://cdn.quasar.dev/logo-v2/svg/logo-mono-white.svg">
          </q-avatar>
          J
        </q-toolbar-title>
      </q-toolbar>
    </q-header> -->

        <q-page-container>
            <!-- <router-view /> -->
            <div class="row q-mt-md">
                <div class="col"> </div>
                <div class="col-6">
                    <q-input outlined v-model="text" />
                    <div class="row justify-center q-mt-sm q-gutter-sm">
                        <q-btn outline 
                        @click="openLink(link)" 
                        :label="name" v-for="{ key, name, link } in search" :key="key"/>
                    </div>
                </div>
                <div class="col"> </div>
            </div>
        </q-page-container>

        <!-- <q-footer bordered class="bg-grey-8 text-white">
      <q-toolbar>
        <q-toolbar-title>
          <q-avatar>
            <img src="https://cdn.quasar.dev/logo-v2/svg/logo-mono-white.svg">
          </q-avatar>
          <div>Title</div>
        </q-toolbar-title>
      </q-toolbar>
    </q-footer> -->

    </q-layout>
</template>