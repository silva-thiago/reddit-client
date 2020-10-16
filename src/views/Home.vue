<template>
  <div class="home">
    <Reddit />
  </div>
</template>

<script>
// @ is an alias to /src
import Reddit from '@/components/Reddit.vue';

export default {
  name: 'Home',
  components: {
    Reddit,
  },
};
</script>
