<template>
  <div class="ebook-reader">
    <div id="read"></div>
  </div>
</template>

<script>
import { ebookMixin } from "../../utils/mixin.js";
import {
  getFontFamily,
  saveFontFamily,
  getFontSize,
  saveFontSize,
  getTheme,
  saveTheme
} from "../../utils/localStorage.js";
import Epub from "epubjs";
import { Promise } from "q";
global.epub = Epub;
export default {
  mixins: [ebookMixin],
  methods: {
    prevPage() {
      if (this.rendition) {
        this.rendition.prev();
        this.hideTitleAndMenu();
      }
    },
    nextPage() {
      if (this.rendition) {
        this.rendition.next();
        this.hideTitleAndMenu();
      }
    },
    toggleTitleAndMenu() {
      // this.$store.dispatch("setMenuVisible", !this.menuVisible);
      if (this.menuVisible) {
        this.setSettingVisible(-1);
        this.setFontFamilyVisible(false);
      }
      this.setMenuVisible(!this.menuVisible);
    },
    hideTitleAndMenu() {
      // this.$store.dispatch("setMenuVisible", false);
      this.setSettingVisible(-1);
      this.setMenuVisible(false);
      this.setFontFamilyVisible(false);
    },
    initFontSize() {
      let fontSize = getFontSize(this.fileName);
      if (!fontSize) {
        saveFontSize(this.fileName, this.defaultFontSize);
      } else {
        this.rendition.themes.fontSize(fontSize);
        this.setDefaultFontSize(fontSize);
      }
    },
    initFontFamily() {
      let font = getFontFamily(this.fileName);
      if (!font) {
        saveFontFamily(this.fileName, this.defaultFontFamily);
      } else {
        this.rendition.themes.font(font);
        this.setDefaultFontFamily(font);
      }
    },
    initTheme() {
      let defaultTheme = getTheme(this.fileName);
      if (!defaultTheme) {
        defaultTheme = this.themeList[0].name;
        saveTheme(this.fileName, defaultTheme);
      }
      this.setDefaultTheme(defaultTheme);
      this.themeList.forEach(theme => {
        this.rendition.themes.register(theme.name, theme.style);
      });
      this.rendition.themes.select(defaultTheme);
    },
    initEpub() {
      const url = process.env.VUE_APP_RES_URL + "/" + this.fileName + ".epub";
      console.log(url);
      this.book = new Epub(url);
      this.setCurrentBook(this.book);
      this.rendition = this.book.renderTo("read", {
        width: innerWidth,
        height: innerHeight,
        method: "default"
      });
      this.rendition.display().then(() => {
        this.initTheme();
        this.initFontSize();
        this.initFontFamily();
        this.initGlobalStyle();
      });
      this.rendition.on("touchstart", event => {
        this.touchStartX = event.changedTouches[0].clientX;
        this.touchStartTime = event.timeStamp;
      });
      this.rendition.on("touchend", event => {
        const offsetX = event.changedTouches[0].clientX - this.touchStartX;
        const time = event.timeStamp - this.touchStartTime;
        console.log(offsetX, time);
        if (time < 500 && offsetX > 40) {
          this.prevPage();
        } else if (time < 500 && offsetX < -40) {
          this.nextPage();
        } else {
          this.toggleTitleAndMenu();
        }
        event.preventDefault();
        event.stopPropagation();
      });
      this.rendition.hooks.content.register(contents => {
        Promise.all([
          contents.addStylesheet(
            `${process.env.VUE_APP_RES_URL}/fonts/cabin.css`
          ),
          contents.addStylesheet(
            `${process.env.VUE_APP_RES_URL}/fonts/daysOne.css`
          ),
          contents.addStylesheet(
            `${process.env.VUE_APP_RES_URL}/fonts/montserrat.css`
          ),
          contents.addStylesheet(
            `${process.env.VUE_APP_RES_URL}/fonts/tangerine.css`
          )
        ]).then(() => {});
      });
    }
  },
  mounted() {
    this.setFileName(this.$route.params.fileName.split("|").join("/")).then(
      () => {
        this.initEpub();
      }
    );
  }
};
</script>

<style lang="sass">

</style>
