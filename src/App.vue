<!-- 這裡寫插槽範例 -->
<template>
  <!-- app代表整個應用程式 -->
  <v-app>
    <!-- v-app-bar 代表應用程式的上方工具列 -->
    <v-app-bar style="background-color: #ff5c36;">
      <v-container class="d-flex align-center">
        <v-app-bar-title>番茄鐘</v-app-bar-title>
        <v-btn prepend-icon="mdi-clock-time-ten-outline" to="/">首頁</v-btn>
        <v-btn prepend-icon="mdi-format-list-checks" to="/list">事項</v-btn>
        <v-btn prepend-icon="mdi-cog-outline" to="/settings">設定</v-btn>
      </v-container>
    </v-app-bar>
    <!-- v-main 代表應用程式的主要內容區域 -->
    <!-- v-main 內容來自 router-view （router-view 內容來自 router/index.js） -->
    <v-main style="background-color: #5f504e;">
      <!--
      <SlotExample v-slot="apple">
        <h1>{{ apple }}</h1>
        <h1>{{ apple.num }}</h1>
        <h1>{{ apple.text }}</h1>
      </SlotExample>
      <SlotExample v-slot="{ num }">
        <h1>{{ num }}</h1>
      </SlotExample>
      -->
      <!--
      <CardA />
      <component :is="CardA" />
      <component :is="CardB" />
      <component :is="c" v-for="(c, i) in components" :key="i" />
      -->
      <!--
        slot = 插槽 = 自訂元件內的某部分 HTML
        v-slot="插槽提供的元件內的變數"
        https://zh-hk.vuejs.org/guide/components/slots.html#scoped-slots
        Component = 目前路由應該顯示的元件
        https://router.vuejs.org/zh/guide/advanced/router-view-slot.html#RouterView-%E6%8F%92%E6%A7%BD
      -->
      <router-view v-slot="{ Component }">
        <!--
          keep-alive 包住的元件不會被銷毀
          include 指定保留的元件
          https://zh-hk.vuejs.org/guide/built-ins/keep-alive.html#include-exclude
        -->
        <!--
          component 動態元件
          is 要使用的元件
        -->
        <keep-alive include="HomeView">
          <component :is="Component" />
        </keep-alive>
      </router-view>
    </v-main>
  </v-app>
</template>

<script setup>
// import CardA from '@/components/CardA.vue'
// import CardB from '@/components/CardB.vue'
// import SlotExample from '@/components/SlotExample.vue'

// const components = [CardA, CardB]
</script>
