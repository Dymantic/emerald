<template>
  <div id="app" class="flex-1">
    <router-view />
  </div>
</template>

<style>
html {
  height: 100%;
}

body {
  background: url("./assets/images/homepage_banner_marble.jpg");
  background-position-y: 100%;
  background-size: cover;
  background-repeat: no-repeat;
}

.gallery-grid {
  display: grid;
  grid-template-columns: repeat(7, 1fr);
  grid-row-gap: 1rem;
  grid-column-gap: 1rem;
}

@media screen and (max-width: 600px) {
  .gallery-grid {
    grid-template-columns: repeat(2, 1fr);
  }
}

.gallery-grid > div {
  position: relative;
  width: 100%;
}

.gallery-grid > div::before {
  content: "";
  display: inline-block;
  width: 1px;
  height: 0;
  padding-bottom: 100%;
}

.fade-enter-active,
.fade-leave-active {
  transition: transform 0.5s;
}
.fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
  transform: scale(0);
}

.slide-enter-active,
.slide-leave-active {
  transition: transform 0.3s;
  transform: translate3d(0, 0, 0);
}
.slide-enter /* .fade-leave-active below version 2.1.8 */ {
  transform: translate3d(-130%, 0, 0);
}

.slide-leave-to {
  transform: translate3d(130%, 0, 0);
}

.slide-right-enter-active,
.slide-right-leave-active {
  transition: transform 0.3s;
  transform: translate3d(0, 0, 0);
}
.slide-right-enter /* .fade-leave-active below version 2.1.8 */ {
  transform: translate3d(130%, 0, 0);
}

.slide-right-leave-to {
  transform: translate3d(-130%, 0, 0);
}
</style>
