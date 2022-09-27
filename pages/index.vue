<template>
  <div class="vcard">
    {{ loading }}
    hello {{ cardId }}
    {{ cardInfo }}
  </div>
</template>

<script>
import axios from 'axios';
import sha1 from 'js-sha1';

export default {
  name: "index.vue",
  computed: {
    cardId() {
      let urlHashValue = this.$route.hash || '';
      urlHashValue = urlHashValue.match(/^#\/card\/([0-9a-f]{5,40})$/)
      return urlHashValue && urlHashValue[1] || ''
    }
  },
  watch: {
    cardId() {
      this.loadCardInfo(this.cardId)
    }
  },
  methods: {

    async loadCardInfo() {
      if (!this.cardId) {
        return;
      }
      this.loading = true;

      try {
        let response = await axios.get(`https://ducati-israel-vcard.s3.eu-central-1.amazonaws.com/${this.cardId}.json`)
        this.cardInfo = response.data
      } catch (e) {
        console.error(e);
      } finally {
        this.loading = false;
      }

    },
  },
  mounted() {
    this.loadCardInfo();
  },


  data() {
    return {
      hello: 'world',
      loading: false,
      cardInfo: {},
    }
  }
}
</script>

<style lang="scss" scoped>

$color: #000;
.vcard {
}

</style>