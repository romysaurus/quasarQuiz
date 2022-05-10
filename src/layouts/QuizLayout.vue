<template>
  <q-layout view="hHh lpR fFf">
    <q-page-container>
      <div id="header">
        <h2>QUIZ TIME</h2>
      </div>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<style lang="scss" scoped>
#header {
  display: flex;
  justify-content: center;
}
</style>
