<template>
  <section class="uiv">
    <label for="input-5">Users of Github:</label>
    <input
      id="input-5"
      class="form-control"
      type="text"
      placeholder="Type to search..."
    />
    <typeahead
      v-model="model"
      target="#input-5"
      :async-function="queryFunction"
      item-key="login"
    >
      <template slot="item" slot-scope="props">
        <li
          v-for="(item, index) in props.items"
          :key="index"
          :class="{ active: props.activeIndex === index }"
        >
          <a role="button" @click="props.select(item)">
            <img width="22px" height="22px" :src="item.avatar_url + '&s=40'" />
            <span v-html="props.highlight(item)"></span>
          </a>
        </li>
      </template>
    </typeahead>
    <br />
    <alert v-show="model">You selected {{ model }}</alert>
  </section>
</template>
<script>
import axios from 'axios' // https://github.com/axios/axios

export default {
  data() {
    return {
      model: '',
    }
  },
  methods: {
    queryFunction(query, done) {
      axios
        .get('https://api.github.com/search/users?q=' + query)
        .then((res) => {
          done(res.data.items)
        })
        .catch((err) => {
          // any error handler
        })
    },
  },
}
</script>
