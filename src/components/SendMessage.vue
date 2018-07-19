<template lang="pug">
  div
    h3 Send Message
    input.form-control(type="text", placeholder="Destination address..", v-model="destination")
    textarea.form-control(rows="4", placeholder="Message..", v-model="message")
    div(style="display: flex; align-items: center; justify-content: flex-end")
      button.btn.btn-outline-secondary(type="button", @click="sendMessage()") SEND
</template>

<script>
import { connectApp } from 'radixdlt-js-lite'

export default {
  name: 'SendMessage',
  props: ['token'],
  data () {
    return {
      destination: '',
      message: ''
    }
  },
  methods: {
    sendMessage () {
      connectApp(this.token)
        .then(radixConnection => {
          const messaging = radixConnection.getMessaging()

          // Send message
          messaging.sendMessage(this.destination, this.message)
            .subscribe(
              response => console.log(`Your message was succesfully submitted: ${response}`),
              error => console.log(`Sorry, something went wrong: ${error}`)
            )
        })
        .catch(error => {
          console.log(`Error while re-using a token: ${error}`)
        })
    }
  }
}
</script>
