<template>
  <q-layout view="hHh lpR fFf">
    <q-header class="header">
      <q-toolbar>
        <q-toolbar-title class="header-text"> QUIZ TIME </q-toolbar-title>
      </q-toolbar>
    </q-header>

    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<style lang="scss" scoped>
.header {
  background-image: radial-gradient(circle, #008793, #00bf72, #a8eb12);
}

.header-text {
  display: flex;
  justify-content: center;
  color: white;
  font-weight: 600;
  font-size: 2.5rem;
}
</style>
