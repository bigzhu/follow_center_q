<template>
  <div class="layout-padding">
    <div class="row gutter sm-column">
      <div class="width-5of5 no-top-padding-bz">
        <div class="ui segment bio-background">
          <img class="responsive" :src='bio.titleImg'>
          <div class="bio-article">
            <h3>{{bio.title}}</h3>
            <div v-html="bio.text">
            </div>
          </div>
          <star-item :star='starInfo' isMy="true" class="bio-star-card"></star-item>
        </div>
      </div>

      <div class="width-2of5 no-top-padding-bz bg">
        <div class="blank-padding"></div>
        <right-info></right-info>
      </div>
    </div>
    <Top></Top>
  </div>
</template>

<script>
  import Top from './Top'
  import StarItem from './StarItem'
  import _ from 'lodash'
  import RightInfo from './RightInfo'
  export default {
    components: {
      Top,
      StarItem,
      RightInfo
    },
    data () {
      return {
        StarName: this.$route.params.StarName
      }
    },
    props: {},
    mounted () {
      this.$store.dispatch('getStar', this.StarName)
      this.getBio()
    },
    computed: {
      starInfo () {
        let starInfo = this.$store.state.starInfos[this.StarName]
        if (starInfo) {
          return starInfo
        }
        return { id: 0, name: '' }
      },
      bio () {
        let self = this
        let bio = _.find(this.$store.state.p.richList, function (d) { return d.key === self.StarName })
        if (bio) return bio
        else return { titleImg: '' }
      }
    },
    methods: {
      getDetail: function () {
        let self = this
        this.$store.dispatch('getRichText', { Key: this.StarName }).then(function (data) {
          self.bio.text = data.richText[0].text
        })
      },
      getBio: function () {
        let self = this
        if (this.bio.titleImg !== '') {
          if (!this.bio.text) { // 没有详情，取之
            this.getDetail()
          }
        } else {
          this.$store.dispatch('getRichList').then(function (data) {
            self.getBio()
          })
        }
      }
    }

  }
</script>

<style scoped>
  .ui.segment.bio-background {
    border-radius: 0.06em;
    box-shadow: .5px 1px 1px 1px rgba(0, 0, 0, 0.1);
    position: relative;
    border: none;
    background-color: #fff;
  }

  .bio-article {
    padding: 1.5rem;
  }

  .bio-star-card.ui.segment.recommand-star-bz {
    box-shadow: none;
    border-top: 1px solid #E6E6E6;
    padding-top: 1rem;
  }
</style>
