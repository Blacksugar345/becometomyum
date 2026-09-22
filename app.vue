<template>
  <ClientOnly>
    <PotCanvas />
  </ClientOnly>
</template>

<style>
@import url('https://fonts.googleapis.com/css2?family=Mali:ital,wght@0,300;0,400;0,500;0,600;0,700;1,400&display=swap');

html, body {
  margin: 0;
  padding: 0;
  width: 100%;
  height: 100%;
  overflow: hidden;
  font-family: 'Mali', cursive, sans-serif;
  background-color: #f4dcb9; /* fallback background */
}

#__nuxt {
  width: 100%;
  height: 100%;
}
</style>