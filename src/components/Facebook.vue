<template>
  <div>
    <div class="description word-wrap-bz" v-html="theText"></div>
    <div class="description word-wrap-bz" v-html="description"></div>
    <a v-show="imgUrl && type!=='video'" @click="openImg(imgUrl)">
      <img :src="proxy(imgUrl)" class="responsive">
    </a>
    <video v-if="type==='video'" :controls="true" type='video/mp4'>
      <source :src="proxy(video)">
    </video>
  </div>
</template>

<script>
  // import '../assets/public.css'
  import myautolinker from '../functions/myautolinker'
  import Proxy from './Proxy'

  export default {
    mixins: [Proxy],
    props: ['message'],
    computed: {
      video: function () {
        return this.message.extendedEntities.source
      },
      type: function () {
        return this.message.type
      },
      imgUrl: function () {
        if (this.message.extendedEntities.pictrue) {
          return this.message.extendedEntities.pictrue
        } else {
          return ''
        }
      },
      description: function () {
        if (this.message.content.description) {
          if (this.message.text) {
            if (this.message.text.includes(this.message.content.description)) return ''
          }
          return myautolinker(this.message.content.description, 'facebook')
        } else {
          return ''
        }
      },
      theText: function () {
        if (this.message.text) {
          return myautolinker(this.message.text, 'facebook')
        }
        return ''
      }
    },
    methods: {
      openImg: function (imgUrl) {
        if (this.$route.name === 'TheMessage') { // 在 TheMessage 还点了图，就在新页中打开图
          window.open(imgUrl, '_blank')
        } else {
          this.$router.push({name: 'TheMessage', params: {id: this.message.id}})
        }
      }
    }
  }
</script>

<style scoped>
  video, img.responsive {
    padding-top: 1rem
  }
  video {
    max-width: 100%;
    max-height: 40rem;
  }
</style>
