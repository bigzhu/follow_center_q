<template>
  <q-page padding>
    <messages type="star" :starName="this.$route.params.StarName"></messages>
    <Top/>
  </q-page>
</template>

<script>
    import Messages from '../components/Messages'
    import Top from '../components/Top'
    export default {
        mounted() {
            this.$store.commit('setNowStarForce', this.$route.params.StarName)
        },
        components: {
            Top,
            Messages
        }
    }
</script>

<style scoped>
  /*
  .blank-padding {
    padding: 18px;
  }

  .row.gutter>.no-top-padding-bz {
    padding-top: 0;
  }
   */
</style>
