<template>
  <div
    class="image"
    v-bind:style="cssObj"
    v-on:mouseover="ishovered = true"
    v-on:mouseleave="ishovered = false">
    <div class="image__wrapper">
      <DeleteBtn
        v-show="ishovered"
        v-on:delete="handleDelete" />
      <img v-bind:src="url" alt="couldn't find the image" ref="img">
    </div>
  </div>
</template>

<script>
import DeleteBtn from './DeleteBtn'
export default {
  name: 'ImageBox',
  components: { DeleteBtn },
  props: {
    id: Number,
    url: String,
    heightWidthProportion: { type: Number, default: 1 / 1 }
  },
  data () {
    return {
      ishovered: false
    }
  },
  methods: {
    fixImageDisplay () {
      const img = new Image()
      img.onload = () => {
        const originalHeight = img.height
        const originalWidth = img.width
        const heightWidthProportion = this.$props.heightWidthProportion
        // console.log(originalHeight, originalWidth, heightWidthProportion)
        if (originalHeight / originalWidth >= heightWidthProportion) {
          this.$refs.img.style.minWidth = '100%'
          this.$refs.img.style.maxWidth = '100%'
        } else {
          this.$refs.img.style.minHeight = '100%'
          this.$refs.img.style.maxHeight = '100%'
        }
      }
      img.src = this.$props.url
    },
    handleDelete () {
      this.$emit('delete', this.$props.id)
    }
  },
  computed: {
    cssObj: function () {
      return {
        '--pseudoElementPaddingBottom': (this.heightWidthProportion * 100) + '%'
      }
    }
  },
  mounted: function () {
    this.fixImageDisplay()
  }
}
</script>

<style scoped>
.image {
  position: relative;
}

.image:before {
  content: '';
  display: block;
  padding-bottom: var(--pseudoElementPaddingBottom);
  background-color: #cc543a50;
}

.image__wrapper {
  position: absolute;
  width: 100%;
  height: 100%;
  top: 0;
  overflow: hidden;
  display: flex;
  align-items: center;
  justify-content: center;
}
</style>
