<template>
  <div class="container" :style="{ fontSize: '12px' }">
    <pre>{{ document }}</pre>
    <hr>
    <pre>{{ slices }}</pre>
    <hr>
    <pre>{{ referrerPath }}</pre>
  </div>
</template>

<script>
const authors = {
  "Sonia Chu": {
    firstname: "Sonia",
    lastname: "Chu"
  }
};

export default {
  async asyncData({ $prismic, error }) {
    const params = {
      uid: "run-netsuite-close-with-blackline"
    };

    try {
      const result = await $prismic.api.getByUID("blog_post", params.uid);
      const document = result.data;
      if (
        Object.prototype.hasOwnProperty.call(authors, document.author)
      ) {
        document.author_title = authors[document.author];
      }
      
      return {
        document,
        slices: result.data.body,
        referrerPath: "/" + params.uid,
      };
    } catch (e) {
      let statusCode = 500;
      if (e.error) {
        statusCode = e.error.statusCode;
      }
      error({
        statusCode,
        message: e.message || "Page not found",
      });
    }
  },
}
</script>