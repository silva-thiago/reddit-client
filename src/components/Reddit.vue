<template>
  <div class="container">
    <div v-if="postsState.loading" class="progress blue lighten-4">
      <div class="indeterminate blue darken-4"></div>
    </div>
    <div class="row">
      <div class="col s12 m6 offset-m3">
        <div v-if="postsState.error" class="card red darken-1 center">
          <div class="card-content white-text">
            <span class="card-title">{{ postsState.error }}</span>
          </div>
        </div>
      </div>
    </div>
    <div class="row">
      <div class="col s12 m8 offset-m2" v-for="post in posts" :key="post.id">
        <div class="card post">
          <div class="card-content">
            <p class="grey-text">Posted by {{ post.author }}</p>
            <blockquote class="card-title grey-text text-darken-4">
              {{ post.title }}
              <!-- <i class="material-icons right">more_vert</i> -->
            </blockquote>
            <!-- Video -->
            <!-- <div class="video-container z-depth-1" v-if="isVideo(post)">
              <video class="video" controls muted autoplay loop>
                <source type="video/mp4" :src="getVideoURL(post)" />
              </video>
            </div> -->
            <!-- Image -->
            <div class="card-image z-depth-1" v-if="isImage(post.url)">
              <img class="responsive-img" :src="post.url" />
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
      </div>
    </div>
  </div>
</template>

<script setup>
import { computed } from 'vue';

import usePosts from '@/hooks/usePosts';

export const postsState = usePosts('all');
export const posts = computed(() => postsState.data.map((child) => child.data));

export function isImage(url) {
  return url.match(/bmp|webp|png|jpg|jpeg|gif$/);
}

export function isVideo(post) {
  return post.media || post.url.match(/mp4|gifv|mkv|mov|webm$/);
}

export function getVideoURL(post) {
  if (post.media) {
    return post.media.reddit_video.scrubber_media_url;
  }
  console.log(post.media);

  const parts = post.url.split('.');

  parts.pop();

  return parts.concat('mp4').join('.');
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.post {
  height: auto;
}

.video {
  width: 100%;
}
</style>
