<template>
  <nav class="nav-wrapper blue">
    <a href="#" class="brand-logo center">
      <i class="material-icons">stars</i>
      Reddit Client
    </a>
    <!-- <ul id="nav-mobile" class="right hide-on-med-and-down">
      <li><a href="sass.html">Sass</a></li>
    </ul> -->
  </nav>
  <div class="container">
    <div class="row">
      <div class="col s12 m8 offset-m2">
        <form @submit.prevent="updateSubreddit" class="">
          <div class="input-field z-depth-1">
            <input
              id="search"
              type="search"
              class="autocomplete"
              ref="subreddit"
              maxlength="2048"
              v-model="searchTerm"
              autocomplete="off"
            />
            <label class="label-icon" for="search">
              <i class="material-icons">search</i>
            </label>
            <i class="material-icons close-icon">close</i>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>

<script>
import state from '@/store/diy';
import { onMounted, ref, watch } from 'vue';

import API from '@/lib/API';

export default {
  setup() {
    const searchTerm = ref('');
    const subreddit = ref(null);

    let instance;
    let debounceTimeout;

    onMounted(() => {
      // eslint-disable-next-line no-undef
      instance = M.Autocomplete.init(subreddit.value, {
        data: {},
        onAutocomplete(result) {
          state.subreddit.value = `r/${result}`;
        },
      });

      // instance.updateData();

      async function getResults() {
        if (searchTerm.value.length < 3) {
          return;
        }

        const response = await API.getPosts('search', {
          q: searchTerm.value,
          type: 'sr',
        });
        const results = response.data.children.reduce((all, child) => {
          if (!child.data.over18) {
            // eslint-disable-next-line no-param-reassign
            all[child.data.display_name] = null;
          }
          return all;
        }, {});

        instance.updateData(results);
        instance.open();
      }

      watch(
        () => searchTerm.value,
        () => {
          clearTimeout(debounceTimeout);
          debounceTimeout = setTimeout(() => {
            getResults();
          }, 100);
        },
      );
    });

    const updateSubreddit = () => {
      clearTimeout(debounceTimeout);
      state.subreddit.value = `r/${searchTerm.value}`;

      if (subreddit.value) {
        instance.close();
        // subreddit.value.blur();
      }
    };

    return {
      updateSubreddit,
      searchTerm,
      subreddit,
    };
  },
};
</script>

<style lang="scss" scoped>
.brand-logo i {
  font-size: 2rem;
  padding: 0.25rem 0 0;
}

form {
  padding: 2rem 0 0;
  width: auto;
  // max-width: 600px;

  .input-field {
    background: white;
    border: 1px solid rgb(223, 225, 229);
    border-radius: 0.5rem;
    box-shadow: none;
    display: flex;
    height: 4.2rem;
    margin: 0 auto;
    padding: 0 2rem 0;
  }

  .autocomplete {
    border: none;
    display: flex;
    font-size: 1.5rem;
    height: 2.25rem;
    padding: 0 2rem 0.75rem;
    margin: 1rem 0 0;
  }

  .label-icon {
    align-items: center;
    display: flex;
    height: 1.5rem;
    margin-top: 0.5rem;
    padding: 0 1rem;
    width: 1.5rem;
  }

  .close-icon {
    top: 1rem !important;
  }
}
</style>
