<template>
  <div class="section">
    <div class="container">
      <div class="columns">
        <div class="column is-offset-2 is-8">
          <p class="subtitle is-6">
            <nuxt-link to="/">Back to blog home</nuxt-link>
          </p>
          <h1 class="title is-2">
            {{ post.fields.title }}
          </h1>
          <hr />
          <div class="content" v-html="$md.render(post.fields.content)"></div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import client from "~/plugins/contentful";
export default {
  asyncData({ params, error, payload }) {
    if (payload) {
      return { post: payload };
    }
    return client
      .getEntries({
        content_type: "post",
        "fields.slug": params.slug
      })
      .then(entries => {
        return { post: entries.items[0] };
      })
      .catch(err => console.log(err));
  },
  head() {
    return {
      title: this.post.fields.title
    };
  }
};
</script>

<style></style>
