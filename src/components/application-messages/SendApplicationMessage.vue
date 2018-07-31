<template lang="pug">
  div
    h3 Send Application Message
    input.form-control(type="text", placeholder="Application ID..", v-model="applicationId")
    input.form-control(type="text", placeholder="Destination addresses (separated by comma)..", v-model="destinations")
    textarea.form-control(rows="5", placeholder="Message..", v-model="message")
    div(style="display: flex; align-items: center; justify-content: flex-end")
      div.form-check(style="margin-right: 20px; margin-bottom: 0")
        input.form-check-input(type="checkbox", value="", id="encryptedCheck", v-model="encrypted")
        label.form-check-label(for="encryptedCheck") Encrypted
      button.btn.btn-outline-secondary(type="button", @click="sendApplicationMessage()") SEND
</template>

<script>
import { connectApp } from 'radixdlt-js-lite'

export default {
  name: 'SendApplicationMessage',
  props: ['token'],
  data () {
    return {
      applicationId: '',
      destinations: '',
      message: '',
      encrypted: true
    }
  },
  methods: {
    sendApplicationMessage () {
      connectApp(this.token)
        .then(radixConnection => {
          const messaging = radixConnection.getMessaging()

          // Send application message
          messaging.sendApplicationMessage(
            this.applicationId,
            this.destinations.split(','),
            this.message,
            this.encrypted
          ).subscribe(
            response => console.log(`Your application message was succesfully submitted: ${response}`),
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
