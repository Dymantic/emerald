<template>
  <div class="h-full relative">
    <router-link to="/" class="p-6 block">
      <full-logo class="text-black hover:text-red-500 h-8"></full-logo>
    </router-link>
    <div class="gallery-grid max-w-4xl mx-auto mt-20">
      <div v-for="(img, index) in images" :key="index" class="bg-black">
        <transition name="fade">
          <img
            @click="setLightBox(index)"
            v-show="img.ready"
            @load="img.ready = true"
            class="object-fit w-full h-full absolute top-0"
            :src="img.thumb"
            alt
          />
        </transition>
      </div>
      <div class="bg-black h-full flex justify-center items-center">
        <svg
          xmlns="http://www.w3.org/2000/svg"
          viewBox="0 0 42.5 20"
          class="fill-current text-white hover:text-red-500 w-4/5"
          @click="setLightBox(imageCount)"
        >
          <path
            d="M19.87 5H.13A.13.13 0 0 1 0 4.87V.13A.13.13 0 0 1 .13 0h19.74a.13.13 0 0 1 .13.13v4.74a.13.13 0 0 1-.13.13M19.87 12.5H.13a.13.13 0 0 1-.13-.13V7.63a.13.13 0 0 1 .13-.13h19.74a.13.13 0 0 1 .13.13v4.74a.13.13 0 0 1-.13.13M19.87 20H.13a.13.13 0 0 1-.13-.13v-4.74A.13.13 0 0 1 .13 15h19.74a.13.13 0 0 1 .13.13v4.74a.13.13 0 0 1-.13.13M37.5 19.87V.13a.13.13 0 0 1 .13-.13h4.74a.13.13 0 0 1 .13.13v19.74a.13.13 0 0 1-.13.13h-4.74a.13.13 0 0 1-.13-.13M30 19.87V.13a.13.13 0 0 1 .13-.13h4.74a.13.13 0 0 1 .13.13v19.74a.13.13 0 0 1-.13.13h-4.74a.13.13 0 0 1-.13-.13M22.5 19.87V.13a.13.13 0 0 1 .13-.13h4.74a.13.13 0 0 1 .13.13v19.74a.13.13 0 0 1-.13.13h-4.74a.13.13 0 0 1-.13-.13"
          />
        </svg>
      </div>
    </div>
    <div
      v-show="showLightBox"
      class="flex justify-center items-center absolute inset w-full h-screen top-0 bg-black md:bg-transparent"
    >
      <div
        class="flex justify-between items-center bg-black max-w-4xl max-h-80vh w-full h-full relative"
      >
        <button
          :disabled="!hasPrev"
          @click="showPrev"
          class="absolute md:static bottom-0 mx-4 text-white text-3xl font-bold"
        >
          &larr;
        </button>
        <div
          class="flex justify-center items-center flex-1 h-full max-h-full overflow-hidden"
          ref="lightbox"
        >
          <transition :name="slideDirection" mode="out-in">
            <img
              v-if="currentIndex !== imageCount"
              class="max-w-full max-h-full"
              :key="lightBoxImg"
              :src="lightBoxImg"
              alt
            />
            <div
              v-if="currentIndex === imageCount"
              class="w-full h-full max-h-4/5 text-white flex flex-col justify-center items-center"
            >
              <h3 class="text-2xl font-bold mb-8">Opening Hours</h3>
              <p class="text-lg font-bold mb-2">Spring/Autumn Semester</p>
              <p>Mon: 12:15 - 21:00</p>
              <p>Tue: 15:00 - 21:00</p>
              <p>Wed: 12:15 - 20:00</p>
              <p>Thurs: 12:15 - 21:00</p>
              <p>Fri: 12:15 - 21:00</p>

              <p class="mt-6 text-lg font-bold">Summer/Winter Camp</p>
              <p>Mon to Fri: 08:30 - 18:30</p>

              <p class="mt-6 text-lg font-bold">Contact</p>
              <p>042 2522 6012</p>
              <p>emeraldfengyuan@gmail.com</p>
            </div>
          </transition>
        </div>
        <button
          :disabled="!hasNext"
          @click="showNext"
          class="absolute md:static bottom-0 right-0 mx-4 text-white text-3xl font-bold"
        >
          &rarr;
        </button>
        <button
          type="button"
          @click="closeLightBox"
          class="bg-none text-3xl text-white font-bold absolute top-0 right-0 m-8"
        >
          &times;
        </button>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import FullLogo from "@/components/Logo";
import gallery from "@/data/images";
import Hammer from "hammerjs";

export default {
  name: "home",
  components: {
    FullLogo
  },

  data() {
    return {
      showLightBox: false,
      currentIndex: 0,
      images: gallery,
      slideDirection: "slide",
      scrollPos: 0
    };
  },

  computed: {
    lightBoxImg() {
      return this.images[this.currentIndex].full;
    },

    imageCount() {
      return this.images.length;
    },

    hasNext() {
      return this.currentIndex < this.imageCount;
    },

    hasPrev() {
      return this.currentIndex > 0;
    }
  },

  mounted() {
    setTimeout(() => (this.ready = true), 1000);
    document.body.classList.remove("overflow-hidden");

    document.body.addEventListener("keyup", this.handleKey);

    const hammertime = new Hammer(this.$refs.lightbox);

    hammertime.on("swipe", this.handleSwipe);
  },

  methods: {
    handleKey({ key }) {
      switch (key) {
        case "ArrowRight":
          this.showLightBox && this.showNext();
          break;
        case "ArrowLeft":
          this.showLightBox && this.showPrev();
          break;
        case "Escape":
          this.closeLightBox();
          break;
      }
    },

    handleSwipe({ deltaX }) {
      if (deltaX < 0) {
        return this.showNext();
      }

      this.showPrev();
    },

    setLightBox(index) {
      this.scrollPos = document.documentElement.scrollTop;
      document.documentElement.scrollTop = 0;
      document.body.classList.add("overflow-hidden");
      this.currentIndex = index;
      this.showLightBox = true;
    },

    closeLightBox() {
      document.body.classList.remove("overflow-hidden");
      document.documentElement.scrollTop = this.scrollPos;
      this.showLightBox = false;
    },

    showNext() {
      this.slideDirection = "slide";
      if (this.currentIndex === this.imageCount) {
        return;
      }

      this.currentIndex++;
    },

    showPrev() {
      this.slideDirection = "slide-right";
      if (this.currentIndex === 0) {
        return;
      }

      this.currentIndex--;
    }
  }
};
</script>

<style scoped>
button:disabled {
  @apply opacity-25;
}
</style>
