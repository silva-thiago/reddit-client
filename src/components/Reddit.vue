<template>
  <SubReddit :name="state.subreddit" />
</template>

<script>
import state from '@/store/diy';
import SubReddit from './SubReddit.vue';

export default {
  components: {
    SubReddit,
  },

  setup() {
    return {
      state,
    };
  },
};
</script>
