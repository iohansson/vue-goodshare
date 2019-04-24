<template>
  <div>
    <div v-if="customBundleComponents">
      <component
        v-for="name in customBundleComponents"
        :key="name"
        :is="name"
        :has_icon="customBundle.hasIcon"
        :has_counter="customBundle.hasCounter"
        :title_social="(customBundle[name] && customBundle[name].titleSocial) || ''"
        :page_url="customBundle.pageUrl"
        :proxy-url="customBundle.facebookProxyUrl"
      >
      </component>
    </div>
    <div v-if="this.$props.bundle === 'world' && !customBundleComponents">
      <vue-goodshare-facebook has_icon has_counter title_social="Facebook" :proxy-url="facebookProxyUrl"></vue-goodshare-facebook>
      <vue-goodshare-linked-in has_icon has_counter title_social="LinkedIn"></vue-goodshare-linked-in>
      <vue-goodshare-tumblr has_icon has_counter title_social="Tumblr"></vue-goodshare-tumblr>
      <vue-goodshare-pinterest has_icon has_counter title_social="Pinterest"></vue-goodshare-pinterest>
      <vue-goodshare-reddit has_icon has_counter title_social="Reddit"></vue-goodshare-reddit>
      <vue-goodshare-twitter has_icon title_social="Twitter"></vue-goodshare-twitter>
      <vue-goodshare-google-plus has_icon title_social="Google Plus"></vue-goodshare-google-plus>
    </div>
    <div v-if="this.$props.bundle === 'ru' && !customBundleComponents">
      <vue-goodshare-facebook has_icon has_counter :proxy-url="facebookProxyUrl"></vue-goodshare-facebook>
      <vue-goodshare-vkontakte has_icon has_counter></vue-goodshare-vkontakte>
      <vue-goodshare-odnoklassniki has_icon has_counter></vue-goodshare-odnoklassniki>
      <vue-goodshare-telegram has_icon></vue-goodshare-telegram>
    </div>
    <div v-if="this.$props.bundle === 'mobile' && !customBundleComponents">
      <vue-goodshare-telegram has_icon title_social="Telegram"></vue-goodshare-telegram>
      <vue-goodshare-viber has_icon title_social="Viber"></vue-goodshare-viber>
      <vue-goodshare-whats-app has_icon title_social="WhatsApp"></vue-goodshare-whats-app>
      <vue-goodshare-line has_icon title_social="LINE"></vue-goodshare-line>
    </div>
  </div>
</template>

<script>
  // Import VueGoodshare social network components
  import VueGoodshareVkontakte from "./providers/Vkontakte.vue"
  import VueGoodshareOdnoklassniki from "./providers/Odnoklassniki.vue"
  import VueGoodshareFacebook from "./providers/Facebook.vue"
  import VueGoodshareGooglePlus from "./providers/GooglePlus.vue"
  import VueGoodshareMoiMir from "./providers/MoiMir.vue"
  import VueGoodshareTwitter from "./providers/Twitter.vue"
  import VueGoodshareLiveJournal from "./providers/LiveJournal.vue"
  import VueGoodshareLinkedIn from "./providers/LinkedIn.vue"
  import VueGoodshareTumblr from "./providers/Tumblr.vue"
  import VueGoodsharePinterest from "./providers/Pinterest.vue"
  import VueGoodshareReddit from "./providers/Reddit.vue"
  
  // Import VueGoodshare mobile messenger components
  import VueGoodshareTelegram from "./providers/Telegram.vue"
  import VueGoodshareViber from "./providers/Viber.vue"
  import VueGoodshareWhatsApp from "./providers/WhatsApp.vue"
  import VueGoodshareLine from "./providers/Line.vue"
  
  export default {
    name: 'VueGoodshare',
    props: {
      bundle: {
        type: String,
        default: 'world',
      },
      customBundle: {
        type: Object,
        default: {},
      },
      facebookProxyUrl: {
        type: String,
        default: null,
      },
    },
    computed: {
      customBundleComponents() {
        return this.customBundle.use
          && typeof this.customBundle.use.length === 'number'
          && this.customBundle.use.map(
            componentName => 'VueGoodshare' + componentName[0].toUpperCase() + componentName.slice(1)
          );
      },
    },
    components: {
      VueGoodshareVkontakte,
      VueGoodshareOdnoklassniki,
      VueGoodshareFacebook,
      VueGoodshareGooglePlus,
      VueGoodshareMoiMir,
      VueGoodshareLiveJournal,
      VueGoodshareLinkedIn,
      VueGoodshareTumblr,
      VueGoodsharePinterest,
      VueGoodshareReddit,
      VueGoodshareTwitter,
      VueGoodshareTelegram,
      VueGoodshareViber,
      VueGoodshareWhatsApp,
      VueGoodshareLine
    }
  }
</script>
