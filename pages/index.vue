<template lang="pug">
.container
  .section
    .content
      h1 瞑想BGM
      p 瞑想に最適なBGMを指定した時間だけ再生することができます。リラックス効果があるので睡眠用や作業用に使うことも可能です。

    .box
      .buttons
        b-button(size="is-small", :disabled="isPlaying", :type="{'is-success': minute === 5}", @click="setMinutes(5)") 5分
        b-button(size="is-small", :disabled="isPlaying", :type="{'is-success': minute === 10}", @click="setMinutes(10)") 10分
        b-button(size="is-small", :disabled="isPlaying", :type="{'is-success': minute === 15}", @click="setMinutes(15)") 15分

        b-button(size="is-small", :disabled="isPlaying", :type="{'is-success': minute === 20}", @click="setMinutes(20)") 20分
        b-button(size="is-small", :disabled="isPlaying", :type="{'is-success': minute === 25}", @click="setMinutes(25)") 25分
        b-button(size="is-small", :disabled="isPlaying", :type="{'is-success': minute === 30}", @click="setMinutes(30)") 30分

        b-button(size="is-small", :disabled="isPlaying", :type="{'is-success': minute === 35}", @click="setMinutes(35)") 35分
        b-button(size="is-small", :disabled="isPlaying", :type="{'is-success': minute === 40}", @click="setMinutes(40)") 40分
        b-button(size="is-small", :disabled="isPlaying", :type="{'is-success': minute === 45}", @click="setMinutes(45)") 45分

        b-button(size="is-small", :disabled="isPlaying", :type="{'is-success': minute === 50}", @click="setMinutes(50)") 50分
        b-button(size="is-small", :disabled="isPlaying", :type="{'is-success': minute === 55}", @click="setMinutes(55)") 55分
        b-button(size="is-small", :disabled="isPlaying", :type="{'is-success': minute === 60}", @click="setMinutes(60)") 60分

      .remaining-time {{ displayTime }}

      div
        b-button(icon-left="stop", type="is-danger", @click="stop", v-if="isPlaying") 終了
        b-button(icon-left="play", type="is-success", outlined, @click="play", v-else) スタート

    .content
      h2 使用BGMについて
      p BGMは著作権フリーの音源をランダムで選択しています。

      b 使用BGM一覧

      ol
        li
          a(href="https://www.youtube.com/watch?v=ZW4hIav2-Zc", target="_blank", rel="noopener noreferrer") One Hour Meditation Music in 432Hz Tuning / Free Download - Free Music for Video

        li
          a(href="https://www.youtube.com/watch?v=X15LLGNkijI", target="_blank", rel="noopener noreferrer") No Copyright Meditation Music 2021 | Background Music Royalty Free

        li
          a(href="https://www.youtube.com/watch?v=vTjb5BSt4S4", target="_blank", rel="noopener noreferrer") No Copyright Music, Morning Mist, 1 Hour Meditation Music | Copyright Free Music

        li
          a(href="https://www.youtube.com/watch?v=pzJzz3S2OiM", target="_blank", rel="noopener noreferrer") No Copyright Meditation Music | Music for Your Project - Copyright Free

    hr

    b-button#form-button(type="is-warning", icon-left="triangle", @click="openContactFrom")
      strong 要望・不具合報告はこちら
</template>

<script>
import ContactForm from '~/components/ContactForm'

export default {
  components: { ContactForm },
  data() {
    return {
      isPlaying: false,
      audio: null,
      audioContext: null,
      remainingTime: 60 * 5,
      timer: null,
      minute: 5,
      audios: [
        'https://static.koeda.me/meditation/audios/a.mp3',
        'https://static.koeda.me/meditation/audios/b.mp3',
        'https://static.koeda.me/meditation/audios/c.mp3',
        // 'https://static.koeda.me/meditation/audios/d.mp3',
        'https://static.koeda.me/meditation/audios/e.mp3',
      ],
    }
  },

  computed: {
    displayTime() {
      const m = Math.floor(this.remainingTime / 60)

      const mm = ('00' + m).slice(-2)
      const s = this.remainingTime % 60
      const ss = ('00' + s).slice(-2)

      return `${mm}:${ss}`
    },
  },

  mounted() {},

  destroyed() {
    this.stop()
  },
  methods: {
    setAudio() {
      const a = this.audios[Math.floor(Math.random() * this.audios.length)]
      this.audio = new Audio(a)
    },

    play() {
      this.isPlaying = true
      this.setAudio()
      this.audio.play()

      this.timer = setInterval(this.countDown, 1000)
    },

    countDown() {
      this.remainingTime = this.remainingTime - 1
      if (this.remainingTime === 0) {
        this.stop()
      }
    },

    stop() {
      this.isPlaying = false

      clearInterval(this.timer)

      if (this.audio) {
        this.audio.pause()
        this.audio = null
      }

      this.setMinutes(this.minute)
    },

    setMinutes(minute) {
      this.minute = minute
      this.remainingTime = 60 * minute
    },

    openContactFrom() {
      this.$buefy.modal.open({
        parent: this,
        component: ContactForm,
      })
    },
  },
}
</script>

<style scoped>
.remaining-time {
  font-size: 40px;
  font-weight: bold;
  font-family: monospace;
}
</style>
