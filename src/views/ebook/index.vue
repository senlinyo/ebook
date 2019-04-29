<template>
  <div class="ebook">
    <ebook-title></ebook-title>
    <ebook-reader></ebook-reader>
    <ebook-menu></ebook-menu>
  </div>
</template>

<script>
import EbookReader from '../../components/ebbok/EbookReader'
import EbookTitle from '../../components/ebbok/EbookTitle'
import EbookMenu from '../../components/ebbok/EbookMenu'
export default {
  components:{
    EbookReader,
    EbookTitle,
    EbookMenu
  }
}
</script>

<style lang="sass" scoped>
 
</style>
