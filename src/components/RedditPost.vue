<template>
  <div class="card post z-depth-0">
    <div class="card-content">
      <p class="grey-text">Posted by {{ post.author }}</p>
      <blockquote class="card-title grey-text text-darken-4">
        {{ post.title }}
        <!-- <i class="material-icons right">more_vert</i> -->
      </blockquote>
      <div class="media">
        <!-- Video -->
        <div class="video-container" v-if="isVideo">
          <video class="responsive-video video" controls muted autoplay loop>
            <source type="video/mp4" :src="videoURL" />
            Your browser does not support the video tag.
          </video>
        </div>
        <!-- Image -->
        <div class="card-image" v-if="isImage">
          <img class="responsive-img image" :src="post.url" />
        </div>
      </div>
      <div class="card-action">
        <span>{{ post.num_comments }} Comments </span>
        <a
          :href="`https://www.reddit.com${post.permalink}`"
          target="_blank"
          rel="noopener noreferrer"
        >
          Post link
        </a>
      </div>
    </div>
  </div>
</template>

<script>
import { computed } from 'vue';

export default {
  props: {
    post: Object,
  },

  // eslint-disable-next-line vue/no-setup-props-destructure
  setup({ post }) {
    // const { post } = props;

    const isImage = computed(() => post.url.match(/bmp|webp|png|jpg|jpeg|gif$/));

    const isVideo = computed(
      () => (post.secure_media && post.secure_media.reddit_video)
        || post.url.match(/mp4|gifv|mkv|mov|webm$/),
    );

    const videoURL = computed(() => {
      if (post.secure_media && post.secure_media.reddit_video) {
        return post.media.reddit_video.fallback_url;
      }

      const parts = post.url.split('.');

      parts.pop();

      return parts.concat('mp4').join('.');
    });

    return {
      isImage,
      isVideo,
      videoURL,
    };
  },
};
</script>

<style scoped lang="scss">
.post {
  border-radius: 0.5rem;
  height: auto;

  .media {
    display: grid;
    padding: 1rem 0;
    place-items: center;

    .video {
      max-height: 480px;
      max-width: 720px;
    }

    .image {
      max-height: 480px;
      width: auto;
    }
  }

  .card-action {
    display: flex;
    justify-content: space-around;
  }
}
</style>
