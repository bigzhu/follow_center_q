<template>
  <q-page padding class="center-container-bz">
    <br>
    <q-chat-message name="bigzhu" avatar="../statics/assets/tou.jpeg" :text="['设置了Anki帐号才能同步数据!']" stamp="4 minutes ago" />
    <q-chat-message :name="oauthInfo.name" :avatar="oauthInfo.picture" :text="['那开始设置吧!']" stamp="3 minutes ago" sent />

    <q-field icon="stars" label="Anki">
      <q-input v-model="anki.userName" :float-label="$t('邮箱')" />
      <q-input v-model="anki.password" type="password" :float-label="$t('密码')" />
    </q-field>
    <br>
    <q-btn outline color="secondary">
      {{$t("设置")}}
    </q-btn>

    <!--
    <q-field icon="permMedia" :label="$t('关注的社交')">
      <MessageConf/>
    </q-field>
    -->

    <footer>
      <div class="footer-content">
        <a href="/about.html">{{ $t("关于 Follow Center") }}</a> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
        <a class="footer-element" href="http://bigzhu.lorstone.com/follow%20center.html">Change Log</a>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
        <a class="footer-element" href="http://bigzhu.lorstone.com/tag/follow.center/index.html">{{ $t("使用教程") }}</a> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;© 2016 Follow Center
      </div>
    </footer>
  </q-page>
</template>

<script>
  // import anki from '../components/anki'
  import Proxy from '../components/Proxy'
  // import MessageConf from '../components/MessageConf'
  export default {
    mixins: [Proxy],
    components: {},
    computed: {
      oauthInfo() {
        return this.$store.state.lib.oauthInfo
      },
      anki() {
        return this.$store.state.anki.anki
      }
    },
    data() {
      return {
        blockCount: 0,
        showAnkiInput: true
      }
    },
    mounted: function () {
      let self = this

      if (this.anki.userName == null) {
        this.$store.dispatch('getAnki').then(function (data) {
          if (self.anki.userName !== null) {
            self.showAnkiInput = false
          }
        })
      } else {
        this.showAnkiInput = false
      }
      this.$store.dispatch('getBlock', {
        Count: true
      }).then(function (data) {
        self.blockCount = data.count
      })
      this.$nextTick(function () {
        // code that assumes this.$el is in-document
      })
    },
    methods: {
      ankiLogin: function () {
        let self = this
        if (this.anki.userName === null || this.anki.userName === '') {
          throw new Error('请填入anki用户名')
        }
        if (this.anki.password === null || this.anki.password === '') {
          throw new Error('请填入anki密码')
        }
        this.$store.dispatch('loginAnki', this.anki).then(function (data) {
          self.showAnkiInput = false
        })
      }
    },
    beforeRouteEnter(to, from, next) {
      next(vm => {
        vm.$store.commit('showRight', false)
      })
    },
    beforeRouteLeave(to, from, next) {
      if (!this.$q.platform.is.mobile) {
        this.$store.commit('showRight', true)
      }
      next()
    }
  }
</script>
