<template>
  <div class="article-body">
    <h1 class="title"> {{title}} </h1>
    <h4 class="time" v-if="updated_at">更新时间：{{updated_at}}</h4>
    <div v-html="body_html" class="body_html"></div>
  </div>
</template>

<script>
import configObj from '@/config.js'
import dayjs from 'dayjs'
import axios from '@/axios.js'

export default {
  name: 'Articles',
  data () {
    return {
      title: '',
      updated_at: '',
      body_html: '',
    }
  },
  mounted() {
    let userId = parseInt(this.$route.query.userId);
    let person = configObj.authorsAndBooks.filter(obj => obj.user_id === userId).shift();
    // console.log(this.$route.params);
    axios.getDocDetail(person.author, person.book, this.$route.params.id)
    .then((response)=>{
      let {title, updated_at, body_html} = response.data.data
      this.title = title
      this.updated_at = dayjs(updated_at).format('YYYY-MM-DD HH:MM:ss')
      this.body_html = body_html
      // console.log(body_html);
    }).catch((response)=>{
      console.log(response);
    })
  }
}
</script>

<style lang="css" scoped>
  .article-body{
    margin-left: 20px;
    max-width: 752px;
    margin: 32px auto 0;
  }
  .time{
    margin: 20px 0;
    color: rgb(38, 38, 38);
    font-weight: 400;
  }
  ::v-deep img{
    border:0;
    vertical-align: middle;
    width: 100%;
    max-width: 100%;
    height: auto;
  }
</style>
