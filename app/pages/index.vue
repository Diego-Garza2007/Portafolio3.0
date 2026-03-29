<template>
  <div class="container">
    <NuxtLink to="/about" class="container__link">
      <Box video-src="/h1.mp4">About</Box>
    </NuxtLink>
    <NuxtLink to="/Projects" class="container__link">
      <Box video-src="/Projects.mp4" align-right>Projects</Box>
    </NuxtLink>
    <ContactMinigame />
<NuxtLink to="/game" class="container__link">
  <Box align-right>
    <template #background>
      <CaveParallax />
    </template>
    Play
  </Box>
</NuxtLink>
    <Circle />
  </div>
</template>
<style lang="scss" scoped>
.container {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  grid-template-rows: repeat(2, 1fr);
  gap: 30px;
  width: 100vw;
  height: 100vh;
  padding: 30px;
  margin: 0;
  position: relative;
  box-sizing: border-box;
  background-color: #f0ede8;
}
.container__link {
  display: block;
  width: 100%;
  height: 100%;
  text-decoration: none;
}
.play-box {
  text-align: right;
  padding-right: 20px; // ajusta el espacio
}
</style>
