<template>
  <ol>
    <li v-for="news of list">
      <p class="title">{{ news.title }}</p>
      <p class="date">{{ news.create_at }}</p>
      <p class="author">By: {{ news.author.loginname }}</p>
    </li>
  </ol>
</template>
<script>
import news from '../api/news'

export default {
  data () {
    return {
      list: [],
      limit: 10
    }
  },
  props: {
    page: {
      type: Number,
      default: 1
    }
  },
  created () {
    this.get()
  },
  watch: {
    page (val) {
      this.get()
    }
  },
  methods: {
    get () {
      news.getList({
        page: this.page,
        limit: this.limit
      }, (err, list) => {
        if (err) {
          console.log(err)
        } else {
          list.data.forEach((data) => {
            const d = new Date(data.create_at)
            data.create_at = `${d.getFullYear()}-${d.getMonth() + 1}-${d.getDate()}`
          })
          this.list = this.list.concat(list.data)
        }
      })
    }
  }
}
</script>

<style scoped>
  ol {
    margin-left: 2rem;
    list-style: outside decimal;
  }
  li {
    line-height: 1.5;
    padding: 1rem;
    border-bottom: 1px solid #b6b6b6;
  }
  .title {
    font-weight: bold;
    font-size: 1.3rem;
  }
  .date {
    font-size: .8rem;
    color: #d6d6d6;
  }
</style>
