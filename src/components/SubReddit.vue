<script lang="jsx">
import { computed } from 'vue';

import usePosts from '@/hooks/usePosts';
import RedditPost from './RedditPost.vue';

export default {
  props: {
    name: Object, // String?
  },

  setup(props) {
    const postsState = usePosts(props.name);

    const posts = computed(() => postsState
      .data
      .filter((child) => !child.data.over_18)
      .map((child) => child.data));

    return {
      postsState,
      posts,
    };
  },

  render() {
    const { postsState, posts } = this;

    return (
      <div class="container">
        {postsState.loading && (
          <div class="progress blue lighten-4">
            <div class="indeterminate blue darken-4"></div>
          </div>
        )}
        {postsState.error && (
        <div class="row">
          <div class="col s12 m6 offset-m3">
              <div class="card red darken-1 center">
                <div class="card-content white-text">
                  <span class="card-title">{postsState.error}</span>
                </div>
              </div>
          </div>
        </div>
        )}
        <div class="row">
          <div class="col s12 m8 offset-m2">
            {posts.map((post) => (
              <RedditPost key={post.id} post={post} />
            ))}
          </div>
        </div>
      </div>
    );
  },
};
</script>
