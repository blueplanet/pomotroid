<template>
</template>

<script>
import { EventBus } from '@/utils/event-bus'
import { ipcRenderer } from 'electron'

export default {
  name: 'Notification',

  data () {
    return {
      notification: null
    }
  },

  methods: {
    callNotification (opts) {
      ipcRenderer.send('show-alert', opts.body)
      console.log('callNotification!!!!!!!!!!!!!!!!!!!')
    },
    notifyLongBreak () {
      this.callNotification({
        title: 'Work Round Complte',
        body: 'Begin a long break.'
      })
    },
    notifyShortBreak () {
      this.callNotification({
        title: 'Work Round Complete',
        body: 'Begin a short break.'
      })
    },
    notifyWork () {
      this.callNotification({
        title: 'Break Finished',
        body: 'Begin working.'
      })
    }
  },

  mounted () {
    EventBus.$on('ready-long-break', this.notifyLongBreak)
    EventBus.$on('ready-short-break', this.notifyShortBreak)
    EventBus.$on('ready-work', this.notifyWork)
  },

  beforeDestroy () {
    EventBus.$off('ready-long-break', this.notifyLongBreak)
    EventBus.$off('ready-short-break', this.notifyShortBreak)
    EventBus.$off('ready-work', this.notifyWork)
  }
}
</script>
