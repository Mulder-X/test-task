<template>
  <div>
    <Header />
    <HexComp />
    <Footer />
  </div>
</template>
<script>
import Footer from "@/layouts/Footer";
import Header from "@/layouts/Header";
import HexComp from "@/components/HexComp";

export default {
  name: "app",
  components: {
    Header,
    Footer,
    HexComp
  }
};
</script>

<style>
</style>
