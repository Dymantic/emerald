<template>
  <div class="flex justify-center items-center h-full">
    <router-link to="/gallery">
      <full-logo class="text-black hover:text-red-500 h-48"></full-logo>
    </router-link>
  </div>
</template>

<script>
// @ is an alias to /src
import FullLogo from "@/components/Logo";
export default {
  name: "home",
  components: {
    FullLogo
  }
};
</script>
