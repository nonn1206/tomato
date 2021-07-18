<template lang="pug">
#home
  b-container
    b-row
      b-col(cols='12')
        h1 {{ timeText }}
        h5 {{ currentText }}
        b-btn.btn-lg(variant='dark' v-if='status !== 1' @click='start' ) Start
          //- font-awesome-icon(:icon='["fas", "play"]')
        b-btn.btn-lg.btn3(variant='primary' v-if='status === 1' @click='pause') ||
          //- h6 Restart
          //- font-awesome-icon(:icon='["fas", "pause"]')
        //- b-btn.btn-lg.btn4(variant='primary' v-if='current.length > 0' @click='finish(true)') ||
        //-   h5 reset
        //-   //- font-awesome-icon(:icon='["fas", "step-forward"]')
</template>

<script>
export default {
  name: 'Home',
  data () {
    return {
      timer: 0
    }
  },
  computed: {
    status () {
      return this.$store.state.status
    },
    list () {
      return this.$store.state.list
    },
    current () {
      return this.$store.state.current
    },
    currentText () {
      let text = this.current
      if (text.length === 0) {
        if (this.list.length === 0) {
          text = 'Round 1'
        } else {
          text = 'Round 1'
        }
      }
      return text
    },
    timeleft () {
      return this.$store.state.timeleft
    },
    timeText () {
      const m = Math.floor(this.timeleft / 60)
      const s = Math.floor(this.timeleft % 60)
      return `${m} : ${s}`
    }
  },
  methods: {
    pause () {
      clearInterval(this.timer)
      this.$store.commit('changeStatus', 2)
    },
    start () {
      if (this.status !== 2 && this.list.length > 0) {
        this.$store.commit('start')
      }
      if (this.current.length > 0) {
        this.$store.commit('changeStatus', 1)
        this.timer = setInterval(() => {
          this.$store.commit('countdown')
          if (this.timeleft <= -1) {
            this.finish(false)
          }
        }, 1000)
      }
    },
    finish (skip) {
      clearInterval(this.timer)
      this.$store.commit('changeStatus', 0)
      this.$store.commit('addFinish')

      if (!skip) {
        const audio = new Audio()
        audio.src = require('../assets/' + this.$store.state.sound)
        audio.play()
      }

      if (this.list.length > 0) {
        this.start()
      } else {
        this.$swal({
          icon: 'success',
          title: '結束'
        })
      }
    }
  }
}
</script>
