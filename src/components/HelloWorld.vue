<template>
  <div class="hello">
    <h1>Diagnose My Browser</h1>
    <div v-if="error">
      <h2>Error!!</h2>
      <pre><code>{{ error }}</code></pre>
    </div>
    <h3>Current $route</h3>
    <div>
      <pre><code>{{ routeResult }}</code></pre>
    </div>
    <h3>Navigator Results</h3>
    <div>
      <pre><code>{{ navigatorResult }}</code></pre>
    </div>
  </div>
</template>

<script lang="ts">
import Vue from "vue";
import { Route } from "vue-router";

export default Vue.extend({
  name: "HelloWorld",
  data() {
    let err = null;
    const nav: { [key: string]: unknown } = {};
    const route: { [key: string]: unknown } = {};

    try {
      for (const prop in window.navigator) {
        const val = window.navigator[prop as keyof Navigator];
        nav[prop] = val;
      }

      nav.plugins = [...window.navigator.plugins].map(pl => pl.name);
      nav.mimeTypes = [...window.navigator.mimeTypes].map(
        pl => `${pl.type}: ${pl.description}`
      );

      for (const prop in this.$route) {
        if (prop !== "matched") {
          route[prop] = this.$route[prop as keyof Route];
        }
      }
    } catch (e) {
      err = e;
    }

    return {
      error:
        err &&
        JSON.stringify(
          {
            message: err.message,
            stack: err.stack
          },
          null,
          2
        ),
      navigatorResult: JSON.stringify(nav, null, 2),
      routeResult: JSON.stringify(route, null, 2)
    };
  }
});
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="stylus">
.hello
  margin: 0 30px;
h3
  margin 40px 0 0

ul
  list-style-type none
  padding 0

li
  display inline-block
  margin 0 10px

a
  color #42b983
pre
  padding: 12px
  white-space: pre-wrap
  text-align: left
  border-radius: 5px
  margin: 1em 0
  background-color: #333
  color: #eee
  font-family: Courier, 'New Courier', monospace

code
  font-size: 16px
</style>
