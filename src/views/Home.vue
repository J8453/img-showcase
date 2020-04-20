<template>
  <div class="container">
    <div class="logo">
      <img src="../assets/messenger.svg" alt="">
    </div>
    <form v-on:submit.prevent="handleSubmit">
      <div class="form__title">OH!</div>
      <div class="form__row">
        <input
          type="text"
          placeholder="輸入 url 以新增圖片（僅接受 .jpg 與 .png）"
          v-model="newUrl">
        <button type="submit">提交</button>
      </div>
      <div class="form__row">
        <transition
          name="fade"
          v-on:after-enter="error = { hasError: false, msg: ''}">
          <Reminder
            v-if="error.hasError"
            v-bind:msg="error.msg"/>
        </transition>
      </div>
    </form>
    <div class="display" ref="display">
      <div
        class="display__info"
        v-if="size">
        The following image(s)'s size is <span>{{ size.width }}</span>px * <span>{{ size.height }}</span>px.
      </div>
      <div
        class="display__image"
        v-for="item in urls"
        v-bind:key="item.id">
        <div class="display__image--container">
          <ImageBox
            v-bind:id="item.id"
            v-bind:url="item.url"
            v-on:firstImageUploaded="size = $event"
            v-on:delete="handleDelete" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Reminder from '../components/Reminder'
import ImageBox from '../components/ImageBox'
export default {
  name: 'App',
  components: { Reminder, ImageBox },
  data () {
    return {
      newUrl: '',
      nextId: 1,
      urls: [],
      size: null,
      error: {
        hasError: false,
        msg: ''
      }
    }
  },
  methods: {
    handleSubmit: function () {
      if (!this.newUrl.match(/\.jpg$|\.png$/i)) {
        this.error = {
          hasError: true,
          msg: '檔案不符合格式規定；請重新提交'
        }
        return
      }
      const newUrls = [...this.urls, {
        id: this.nextId,
        url: this.newUrl
      }]
      this.urls = newUrls
      this.newUrl = ''
      this.nextId += 1
    },
    getSize: function () {
      const firstImageWrapperElem = document.querySelector('.image__wrapper')
      if (!firstImageWrapperElem) return
      const { offsetWidth: width, offsetHeight: height } = firstImageWrapperElem
      this.size = { width, height }
    },
    handleDelete: function (id) {
      this.urls = this.urls.filter(item => item.id !== id)
    }
  },
  mounted: function () {
    this.$watch('urls', function (val, oldVal) {
      if (val.length === 1 && oldVal.length === 0) {
        this.getSize()
      } else if (val.length === 0) {
        this.size = null
      }
    })
    window.addEventListener('resize', this.getSize)
  },
  beforeDestroy: function () {
    window.removeEventListener('resize', this.getSize)
  }
}
</script>

<style scoped>
.container>* {
  display: flex;
  flex-wrap: wrap;
  margin-top: 10px;
}

.logo {
  justify-content: center;
}

.logo>img {
  margin: 30px 0;
  width: 150px;
}

form>* {
  width: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 5px 10px;
}

form>.form__title {
  font-size: 2em;
  text-shadow: 0 0 5px #004585;
}

form>.form__row>* {
  font-size: 0.9em;
}

form>.form__row>input {
  flex: 0 1 600px;
  background-color: transparent;
  margin-right: 5px;
  border: 1px solid #fff;
  border-radius: 4px;
  padding: 10px;
}

input::placeholder {
  color: #fff;
}

form>.form__row>input:focus {
  outline: none;
  box-shadow: inset 0 0 5px #004585, 0 0 5px #004585;
}

form>.form__row>button {
  flex: 0 0 auto;
  color: #fff;
  background-color: transparent;
  border: 1px solid #fff;
  border-radius: 4px;
  padding: 10px;
  transition: all 0.1s;
}

form>.form__row>button:hover {
  background-color: #00458580;
  box-shadow: inset 0 0 5px #004585, 0 0 5px #004585;
}

@media (min-width: 1001px) {
  .container {
    width: 90%;
  }
  .display__image {
    flex-basis: 33.3%;
  }
}

@media (min-width: 551px) and (max-width: 1000px) {
  .container {
    width: 80%;
  }
  .display__image {
    flex-basis: 50%;
  }
}

@media (max-width: 750px) {
  .display__image {
    flex-basis: 100%;
  }
}

.display {
  display: flex;
}

.display__info {
  flex-basis: 100%;
  text-align: center;
  margin: 15px 0;
}

.display__info>span {
  background-color: #00458590;
}

.display__image {
  padding: 5px;
}

.display__image--container {
  padding: 5px;
  border: 1px solid #00000030;
  border-radius: 4px; 
  background-color: #ffffff99; 
}
</style>