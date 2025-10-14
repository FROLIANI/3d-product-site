<template>
  <router-view v-slot="{ Component, route }" :key="$route.path">
    <transition
      :name="typeof route.meta.transition === 'string' ? route.meta.transition : 'fade'"
      mode="out-in"
    >
      <component :is="Component" />
    </transition>
  </router-view>
</template>
