<template lang="html">
  <div ref="photoSwipe" class="pswp" tabindex="-1" role="dialog" aria-hidden="true">
      <div class="pswp__bg"></div>
      <div class="pswp__scroll-wrap">
          <div class="pswp__container">
              <div class="pswp__item"></div>
              <div class="pswp__item"></div>
              <div class="pswp__item"></div>
          </div>
          <div class="pswp__ui pswp__ui--hidden">
              <div class="pswp__top-bar">

                  <div class="pswp__counter"></div>

                  <button class="pswp__button pswp__button--close" title="Close (Esc)"></button>

                  <button class="pswp__button pswp__button--share" title="Share"></button>

                  <button class="pswp__button pswp__button--fs" title="Toggle fullscreen"></button>

                  <button class="pswp__button pswp__button--zoom" title="Zoom in/out"></button>

                  <div class="pswp__preloader">
                      <div class="pswp__preloader__icn">
                        <div class="pswp__preloader__cut">
                          <div class="pswp__preloader__donut"></div>
                        </div>
                      </div>
                  </div>
              </div>

              <div class="pswp__share-modal pswp__share-modal--hidden pswp__single-tap">
                  <div class="pswp__share-tooltip"></div>
              </div>

              <button class="pswp__button pswp__button--arrow--left" title="Previous (arrow left)">
              </button>

              <button class="pswp__button pswp__button--arrow--right" title="Next (arrow right)">
              </button>

              <div class="pswp__caption">
                  <div class="pswp__caption__center"></div>
              </div>
          </div>
      </div>
  </div>
</template>

<script>
  import PhotoSwipe from './dist/photoswipe/dist/photoswipe';
  import UI from './dist/photoswipe/dist/photoswipe-ui-default';

  export default {
    props: {
      list: {
        type: Array,
        default: [],
      },
      options: {
        type: Object,
        default: {},
      },
    },
    data() {
      return {
        slides: [],
      };
    },
    computed: {},
    methods: {
      init() {
        const options = {
          history: false,
          index: 0,
        };
        for (const key in this.options) {
          if (this.options.hasOwnProperty(key)) {
            options[key] = this.options[key];
          }
        }
        window.photoswipe = this.photoswipe = new PhotoSwipe(this.$el, UI, this.slides, options);
        this.photoswipe.init();
        const eventNames = [
          'beforeChange',
          'afterChange',
          'imageLoadComplete',
          'resize',
          'gettingData',
          'mouseUsed',
          'initialZoomIn',
          'initialZoomInEnd',
          'initialZoomOut',
          'initialZoomOutEnd',
          'parseVerticalMargin',
          'close',
          'unbindEvents',
          'destroy',
          'updateScrollOffset',
          'preventDragEvent',
          'shareLinkClick',
        ];
        for (let i = 0, item; item = eventNames[i++];) {
          this.photoswipe.listen(item, (...args) => {
            args.unshift(item);
            this.$emit.apply(this, args);
          });
        }
        this.photoswipe.framework.bind(this.photoswipe.scrollWrap, 'galleryTap', (e) => {
          this.$emit('galleryTap', e);
        });
      },
    },
    watch: {
      list() {
        this.slides = this.list;
        this.init();
      },
    },
    created() {
      this.slides = this.list;
    },
    mounted() {
      this.init();
    },
    destroyed() {},
  };
</script>

<style lang="scss">
  @import './dist/photoswipe.css';
  @import './dist/default-skin/default-skin.css';
</style>
