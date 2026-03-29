<script>
import '@/assets/styles/about.scss'
</script>

<template>
  <main class="about-page">

    <div class="back-arrow">
      <NuxtLink to="/" class="back-link">
        <Icon name="mdi:arrow-left" size="16" />
        Volver
      </NuxtLink>
    </div>

    <div class="about-page__container">

      <section class="about-page__section about-page__section--intro">
        <h2 class="about-page__title">About Me</h2>
        <p class="about-page__text">
          I am Diego Emilio Garza Gudino, a full-stack web developer focused on
          Frontend development, always eager to learn new technologies and best
          practices to create modern, fast, accessible, and secure websites.
        </p>
        <p class="about-page__text">
          I am passionate about optimizing site performance and ensuring that the
          user experience is intuitive and accessible to the widest possible
          audience.
        </p>
      </section>

      <Techs />

      <section class="about-page__section about-page__section--hobbies">
        <h2 class="about-page__title">Hobbies</h2>
        <ul class="about-page__list">
          <li class="about-page__list-item">Reading books</li>
          <li class="about-page__list-item">Exploring new technologies and frameworks</li>
          <li class="about-page__list-item">Playing strategy and competitive video games</li>
          <li class="about-page__list-item">Learn about cybersecurity and hacking</li>
        </ul>
      </section>

      <section class="about-page__section about-page__section--goals">
        <h2 class="about-page__title">Future Goals</h2>
        <p class="about-page__text">
          My goal is to become a highly skilled software engineer, participate in
          open-source projects, and contribute to the tech community. I also aspire
          to work on large-scale applications, improve my expertise in
          cybersecurity, and continue growing as a professional in the world of
          programming.
        </p>
      </section>

      <SetupEvolution />

    </div>
  </main>
</template>