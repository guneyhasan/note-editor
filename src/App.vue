<template>
  <div class="flex w-screen h-screen text-gray-700">
    <SideBar/>

    <main-content/>
  </div>
</template>

<script>

import MainContent from "@/components/main-content";
import SideBar from "@/components/SideBar";

export default {
  name: 'App',
  components: {SideBar, MainContent, },
}
</script>
