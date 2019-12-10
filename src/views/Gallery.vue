<template>
  <div class="h-full relative flex flex-col justify-center items-center">
    <router-link to="/" class="p-6 block absolute top-0 left-0">
      <full-logo class="text-black hover:text-grey h-8"></full-logo>
    </router-link>
    <div class="gallery-grid w-full max-w-4xl mx-auto mt-20 lg:mt-0 p-4 md:p-0">
      <div v-for="(img, index) in images" :key="index" class="bg-black relative overflow-hidden">
        <transition name="fade">
          <img
            @click="setLightBox(index)"
            v-show="img.ready"
            @load="img.ready = true"
            class="object-fit h-full w-full absolute top-0 cursor-pointer thumb"
            :src="img.thumb"
            alt
          />
        </transition>
      </div>
      <div class="bg-black h-full flex justify-center items-center">
        <img
          @click="setLightBox(images.length)"
          src="/images/em_contact_pattern.png"
          alt="Emerald English logo pattern"
          class="contact-pattern w-full h-full object-contain opacity-0 hover:opacity-100 cursor-pointer"
        />
      </div>
    </div>
    <div
      v-show="showLightBox"
      class="flex justify-center items-center absolute inset w-full h-screen top-0 bg-black"
    >
      <div
        class="flex justify-between items-center bg-black max-w-4xl max-h-screen md:max-h-80vh w-full h-full relative"
      >
        <prev-button @click.native="showPrev"></prev-button>
        <div
          class="flex justify-center items-center flex-1 h-full max-h-full overflow-hidden relative"
          ref="lightbox"
        >
          <transition name="fade">
            <img
              v-if="currentIndex !== imageCount"
              class="max-w-full max-h-full absolute"
              :class="{'opacity-0': showLightBox === false}"
              :key="lightBoxImg"
              :src="lightBoxImg"
              alt
            />
            <contact-details v-if="currentIndex === imageCount"></contact-details>
          </transition>
        </div>
        <next-button @click.native="showNext"></next-button>
      </div>
      <p
        class="absolute top-0 center-x mt-2 bg-tint text-white p-2 font-bold"
      >{{ lightboxPosition }}</p>
      <close-button @click.native="closeLightBox"></close-button>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import ContactDetails from "@/components/ContactDetails";
import NextButton from "@/components/NextButton";
import PrevButton from "@/components/PrevButton";
import CloseButton from "@/components/CloseButton";
import FullLogo from "@/components/Logo";
import gallery from "@/data/images";
import Hammer from "hammerjs";

export default {
  name: "home",
  components: {
    FullLogo,
    ContactDetails,
    NextButton,
    PrevButton,
    CloseButton
  },

  data() {
    return {
      showLightBox: false,
      currentIndex: 0,
      images: gallery,
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

    lightboxPosition() {
      return `${this.currentIndex + 1} of ${this.images.length + 1}`;
    }
  },

  mounted() {
    setTimeout(() => (this.ready = true), 1000);
    document.body.classList.remove("overflow-hidden");

    document.body.addEventListener("keyup", this.handleKey);

    const hammertime = new Hammer(this.$refs.lightbox);

    hammertime.on("swipe", this.handleSwipe);

    this.preloadImages();
  },

  watch: {
    $route(to) {
      if (
        (to.query.lightbox === "false" || !to.query.lightbox) &&
        this.showLightBox
      ) {
        return this.closeLightBox();
      }

      if (to.query.lightbox === "true" && !this.showLightBox) {
        this.$router.replace({ path: "gallery", query: { lightbox: false } });
      }
    }
  },

  methods: {
    preloadImages() {
      this.images.forEach(image => {
        const img = new Image();
        img.src = image.full;
      });
    },

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
      if (deltaX > 0) {
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
      this.$router.push({ path: "gallery", query: { lightbox: true } });
    },

    closeLightBox() {
      document.body.classList.remove("overflow-hidden");
      document.documentElement.scrollTop = this.scrollPos;
      this.showLightBox = false;
      this.slideDirection = "";
      if (this.$route.query.lightbox) {
        this.$router.back();
      }
    },

    showNext() {
      if (this.currentIndex === this.imageCount) {
        return (this.currentIndex = 0);
      }

      this.currentIndex++;
    },

    showPrev() {
      if (this.currentIndex === 0) {
        return (this.currentIndex = this.imageCount);
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

.contact-pattern {
  transition: opacity 0.3s;
}

.thumb {
  transition: 0.7s;
}

.thumb:hover {
  transform: scale(1.3);
}

.scale-on-hover {
  transition: 0.3s;
}

.scale-on-hover:hover {
  @media screen and (min-width: 700px;) {
    transform: scale(1.2);
  }
}

.center-y {
  top: 50%;
  transform: translateY(-50%);
}

.center-x {
  left: 50%;
  transform: translateX(-50%);
}
</style>
