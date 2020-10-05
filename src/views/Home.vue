<template>
  <div class="home">
    <!-- Header Section Begin -->
    <Header />
    <!-- Header End -->

    <!-- Hero Section Begin -->
    <Hero />
    <!-- Hero Section End -->

    <!-- Women Banner Section Begin -->
    <Banner />
    <!-- Women Banner Section End -->

    <!-- Partner Logo Section Begin -->
    <Partner />
    <!-- Partner Logo Section End -->

    <!-- Footer Section Begin -->
    <Footer />
    <!-- Footer Section End -->
  </div>
</template>

<script>
// @ is an alias to /src
import Header from '@/components/Header.vue'
import Hero from '@/components/Hero.vue'
import Banner from '@/components/Banner.vue'
import Partner from '@/components/Partner.vue'
import Footer from '@/components/Footer.vue'

export default {
  name: 'Home',
  components: {
    Header,
    Hero,
    Banner,
    Partner,
    Footer
  }
}
</script>
