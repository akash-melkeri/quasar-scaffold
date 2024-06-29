<template>
  <div class="tw-flex tw-flex-col tw-h-full">
    <div class="tw-shrink-0 tw-font-bold tw-text-2xl poppins tw-flex tw-gap-2 tw-p-4 tw-items-center tw-border-b tw-border-b-white/20">
      <div>
        <svg xmlns="http://www.w3.org/2000/svg" class="tw-h-6 tw-w-6 " viewBox="0 0 128 128"><path fill="currentColor" d="M73.273 64a9.274 9.274 0 1 1-18.548-.001A9.274 9.274 0 0 1 73.273 64m31.891-23.773a47 47 0 0 0-7.289-9.528l-10.727 6.188a35.9 35.9 0 0 0-11.187-6.489a44 44 0 0 0-8.66 12.364C79.238 41.949 91.562 46.273 103 55.21l6.75-3.898a47.3 47.3 0 0 0-4.586-11.086ZM64 111.523a47.6 47.6 0 0 0 11.887-1.546V97.602a35.7 35.7 0 0 0 11.215-6.454a44.2 44.2 0 0 0-6.375-13.687c-5.266 10.738-15.165 19.25-28.625 24.687v7.79A46.2 46.2 0 0 0 64 111.522ZM22.836 40.238a47.2 47.2 0 0 0-4.598 11.075L28.961 57.5c-.793 4.273-.8 8.66-.023 12.938a44 44 0 0 0 15.039 1.324C37.3 61.836 34.887 49 36.898 34.625l-6.75-3.898a47 47 0 0 0-7.312 9.511M64 .125C28.723.125.125 28.723.125 64S28.723 127.875 64 127.875S127.875 99.277 127.875 64S99.277.125 64 .125m0 8.176c30.71 0 55.7 24.988 55.7 55.699S94.71 119.7 64 119.7S8.3 94.71 8.3 64S33.29 8.3 64 8.3Zm0 8.16a47.8 47.8 0 0 0-11.887 1.55v12.376a35.6 35.6 0 0 0-11.215 6.449a44.1 44.1 0 0 0 6.375 13.687c5.266-10.734 15.164-19.25 28.625-24.687V18.05A47.2 47.2 0 0 0 64 16.46ZM89.09 56.1a45 45 0 0 0-5.05.137c6.67 9.926 9.085 22.762 7.073 37.137l6.75 3.898a48 48 0 0 0 7.301-9.511c2-3.473 3.535-7.2 4.598-11.075L99.05 70.5c.8-4.324.8-8.7.023-12.938a44 44 0 0 0-9.984-1.46ZM25 72.774l-6.75 3.903a48.2 48.2 0 0 0 4.586 11.086a47 47 0 0 0 7.289 9.527l10.727-6.187a35.8 35.8 0 0 0 11.187 6.484a44 44 0 0 0 8.66-12.36C48.762 86.04 36.438 81.712 25 72.774m0 0"/></svg>
      </div>
      <div>Quasar Scaffold</div>
    </div>
    <div class="tw-grow tw-flex tw-flex-col">
      <div class="tw-shrink-0 tw-p-4">
        <q-input placeholder="Search" v-model="search" borderless class="" dense>
          <template v-slot:append>
            <q-icon name="search"></q-icon>
          </template>
        </q-input>
      </div>
      <div class="tw-grow">
        <q-scroll-area style="height: 100%;" class="tw-shadow-inner">
          <q-list>
            <template :key="category.label" v-for="category in tools_by_category">
              <q-item-label header dense>
                {{ category.label }}
              </q-item-label>
              <q-item dense @click="$router.push(tool.path)" :key="tool.title" v-for="tool,index in category.tools" clickable v-ripple active-class="tw-font-bold tw-text-white tw-bg-white/20" class="tw-text-gray-400 tw-mx-4 tw-my-3 tw-p-2.5 tw-rounded-xl" :class="index==0?'tw-mb-3':index==category.tools.length-1?'tw-mt-3':'tw-my-3'"  :active="$route.path == tool.path">
                <q-item-section dense side>
                  <svg xmlns="http://www.w3.org/2000/svg" class="tw-w-5 tw-h-5" viewBox="0 0 24 24" v-html="tool.icon"></svg>
                </q-item-section>
                <q-item-section dense label class="">
                  {{ tool.title }}
                </q-item-section>
              </q-item>
            </template>
          </q-list>
        </q-scroll-area>
      </div>
    </div>
    <div class="tw-shrink-0 tw-text-center tw-p-4 tw-w-full">
      <span class="tw-text-gray-400 tw-font-bold">Made with <i class="q-icon tw-text-red-500 notranslate material-icons" aria-hidden="true" role="presentation"
        style="font-size: 18px;">favorite</i> by <span class="tw-bg-gradient-to-r tw-from-white tw-via-white/90 tw-to-white/80 tw-inline-block tw-text-transparent tw-bg-clip-text tw-cursor-pointer ">Phantom</span></span>
    </div>
  </div>
</template>

<script>
import { defineComponent, ref } from 'vue'

export default defineComponent({
  name: 'DrawerPage',
  setup(){
    return{
      search:ref(''),
      tools_by_category:[
        {
          label:'Inputs',
          tools:[
            {
              path: '/',
              title: 'Generate Form',
              icon: `<path fill="currentColor" d="M20 20H4c-1.1 0-2 .9-2 2s.9 2 2 2h16c1.1 0 2-.9 2-2s-.9-2-2-2zM7.11 17c.48 0 .91-.3 1.06-.75l1.01-2.83h5.65l.99 2.82c.16.46.59.76 1.07.76c.79 0 1.33-.79 1.05-1.52L13.69 4.17C13.43 3.47 12.75 3 12 3s-1.43.47-1.69 1.17L6.06 15.48c-.28.73.27 1.52 1.05 1.52zm4.83-11.4h.12l2.03 5.79H9.91l2.03-5.79z"/>`,
            },
          ],
        },
      ],
    }
  }
})
</script>
