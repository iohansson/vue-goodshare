<template>
  <a class="button-social"
     :class="buttonSocialDesignObject"
     :page-url="page_url"
     :page-title="page_title"
     :page-description="page_description"
     :page-image="page_image"
     :button-design="button_design"
     :title-social="title_social"
     :has-square-edges="has_square_edges"
     :has-icon="has_icon"
     :has-counter="has_counter"
     @click.prevent="showShareWindow"
  >
    <i class="icon-vkontakte" v-if="this.$props.has_icon"></i>
    <span class="title-social" v-if="this.$props.title_social">{{ title_social }}</span>
    <counter class="counter-vkontakte"
      v-if="this.$props.has_counter"
      :count="counter_vkontakte"
    />
  </a>
</template>

<script>
  import Counter from '../components/Counter.vue';
  import defaultHref from '../helpers/defaultHref';
  // Variables
  const description = document.querySelector('meta[name="description"]')
  const image = document.querySelector('link[rel="apple-touch-icon"]')
  
  export default {
    name: 'VueGoodshareVkontakte',
    components: {
      Counter,
    },
    props: {
      page_url: {
        type: String,
        default: defaultHref(),
      },
      page_title: {
        type: String,
        default: document.title
      },
      page_description: {
        type: String,
        default: (description) ? description.content : ''
      },
      page_image: {
        type: String,
        default: (image) ? image.src : ''
      },
      button_design: {
        type: String,
        default: 'flat'
      },
      title_social: String,
      has_icon: Boolean,
      has_square_edges: Boolean,
      has_counter: Boolean
    },
    data () {
      return {
        buttonSocialDesignObject: {
          'button-social__square_edges': this.$props.has_square_edges,
          'vkontakte__design__flat': this.$props.button_design === 'flat',
          'vkontakte__design__gradient': this.$props.button_design === 'gradient',
          'vkontakte__design__outline': this.$props.button_design === 'outline'
        },
        counter_vkontakte: 0
      }
    },
    methods: {
      /**
       * Get a random integer between `min` and `max`.
       *
       * @param {number} min - min number
       * @param {number} max - max number
       * @return {number} a random integer
       */
      getRandomInt: (min, max) => {
        return Math.floor(Math.random() * (max - min + 1) + min)
      },
      
      /**
       * Slice thousand integer and add `k` letter.
       *
       * @param {number} number - thousand integer
       * @return {string} a integer with letter
       */
      sliceThousandInt: (number) => {
        return (number / 1000).toFixed(1) + 'k'
      },
      
      /**
       * Show share window.
       *
       * @return {object} a pop-up window
       */
      showShareWindow: function () {
        // Variables
        const width = 640
        const height = 640
        let left = (screen.width / 2) - (width / 2)
        let top = (screen.height / 2) - (height / 2)
        const window_config = 'width=' + width + ',height=' + height + ',left=' + left + ',top=' + top + ','
        const share_url = 'https://vk.com/share.php?'
          + 'url=' + encodeURIComponent(this.$props.page_url)
          + '&title=' + encodeURIComponent(this.$props.page_title)
          + '&description=' + encodeURIComponent(this.$props.page_description)
          + '&image=' + encodeURIComponent(this.$props.page_image)
          + '&noparse=true'
        
        return window.open(share_url, 'Share this', window_config + 'toolbar=no,menubar=no,scrollbars=no')
      },
      
      handleUpdateCount(count) {
        this.counter_vkontakte = (count >= 1000)
          ? this.sliceThousandInt(count)
          : count;
      },

      /**
       * Get share counter.
       *
       * @return {object} a share counter
       */
      getShareCounter: function () {
        // Let's see whether some other component has already
        // asked for count. Then we just subscribe for the count update event
        if (window.VK && window.VK.Share && typeof window.VK.Share.count === 'function') {
          return;
        }
        // Variables
        const script = document.createElement('script')
        
        // Create `script` tag with share count URL
        script.src = 'https://vk.com/share.php?act=count'
          + '&index=' + this.getRandomInt(1, 2345)
          + '&url=' + encodeURIComponent(this.$props.page_url)
        
        // Add `script` tag with share count URL
        // to end of `body` tag
        document.body.appendChild(script)
        
        // Set share count to `counter_vkontakte` v-model
        window.VK = Object.assign({}, { Share: {} }, window.VK);
        window.VK.Share.count = (index, count) => {
          if (count) {
            this.$root.$emit('VK:Share:count:update', count);
          }
        }
      }
    },
    mounted () {
      // Show share counter when page loaded
      if (this.$props.has_counter) this.getShareCounter();
      // Subscribe to update count event using $root as an event bus
      this.$root.$on('VK:Share:count:update', this.handleUpdateCount.bind(this));
    },
    destroyed() {
      this.$root.$off('VK:Share:count:update', this.handleUpdateCount.bind(this));
    }
  }
</script>

<style scoped lang="scss">
  // Fontello
  @font-face {
    font-family: 'Fontello';
    src: url('../fonts/fontello.eot?88483298');
    src: url('../fonts/fontello.eot?88483298#iefix') format('embedded-opentype'),
    url('../fonts/fontello.woff2?88483298') format('woff2'),
    url('../fonts/fontello.woff?88483298') format('woff'),
    url('../fonts/fontello.ttf?88483298') format('truetype'),
    url('../fonts/fontello.svg?88483298#fontello') format('svg');
    font-weight: normal;
    font-style: normal;
  }
  
  [class^="icon-"]:before, [class*=" icon-"]:before {
    font-family: 'Fontello';
    font-style: normal;
    font-weight: normal;
    speak: none;
    display: inline-block;
    text-decoration: inherit;
    width: 1em;
    margin-right: .2em;
    text-align: center;
    font-variant: normal;
    text-transform: none;
    line-height: 1em;
    margin-left: .2em;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
  }
  
  .icon-vkontakte:before {
    content: '\e800';
  }
  
  // Colors
  $vkontakte_main_color: rgb(76, 117, 163);
  $vkontakte_main_color_opacity: rgba(76, 117, 163, .5);
  $gradient_color: rgb(106, 147, 193);
  $background_white_color: rgb(254, 254, 254);
  $text_white_color: rgb(254, 254, 254);
  $text_white_color_opacity: rgba(254, 254, 254, .5);
  
  // Reset
  .button-social * {
    box-sizing: border-box;
  }
  
  // Button Social link style
  .button-social {
    display: inline-flex;
    cursor: pointer;
    padding: 7px 10px;
    margin: 3px 1.5px;
    border-radius: 3px;
    -moz-border-radius: 3px;
    -webkit-border-radius: 3px;
  }
  
  // Button Social link style on hover
  .button-social:hover {
    opacity: .9;
  }
  
  // Button Social round edges
  .button-social__square_edges {
    border-radius: 0;
    -moz-border-radius: 0;
    -webkit-border-radius: 0;
  }
  
  // Button Vkontakte style `flat`
  .vkontakte__design__flat {
    background-color: $vkontakte_main_color;
    color: $text_white_color;
  }
  
  // Button Vkontakte style `gradient`
  .vkontakte__design__gradient {
    background-image: linear-gradient(bottom, $vkontakte_main_color, $gradient_color);
    background-image: -moz-linear-gradient(bottom, $vkontakte_main_color, $gradient_color);
    background-image: -o-linear-gradient(bottom, $vkontakte_main_color, $gradient_color);
    background-image: -webkit-linear-gradient(bottom, $vkontakte_main_color, $gradient_color);
    background-image: -ms-linear-gradient(bottom, $vkontakte_main_color, $gradient_color);
    color: $text_white_color;
  }
  
  // Button Vkontakte style `outline`
  .vkontakte__design__outline {
    background-color: $background_white_color;
    border: 1px solid $vkontakte_main_color;
    color: $vkontakte_main_color;
  }
  
  // Title
  .title-social {
    margin-left: 5px;
  }
  
  // Counter
  .counter-vkontakte {
    margin-left: 6px;
    padding-left: 6px;
  }
  
  .vkontakte__design__flat .counter-vkontakte,
  .vkontakte__design__gradient .counter-vkontakte {
    border-left: 1px solid $text_white_color_opacity;
  }
  
  .vkontakte__design__outline .counter-vkontakte {
    border-left: 1px solid $vkontakte_main_color_opacity;
  }
</style>
