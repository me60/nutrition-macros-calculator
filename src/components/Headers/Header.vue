<template>
  <div id="header">
      <HeaderTopBanner />
      <HeaderBody />
      <HeaderBottomBanner />
  </div>
</template>

<script>
    import HeaderTopBanner from './HeaderTopBanner.vue';
    import HeaderBody from './HeaderBody.vue';
    import HeaderBottomBanner from './HeaderBottomBanner.vue';

    export default {
        name: 'Header',
        components: {
            HeaderTopBanner,
            HeaderBottomBanner,
            HeaderBody
        }
    }
</script>

<style>
</style>
