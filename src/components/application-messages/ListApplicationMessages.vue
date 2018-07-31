<template lang="pug">
  div
    h3 Application Messages
    div.input-group.mb-3
      input.form-control(type="text", placeholder="Application ID..", v-model="applicationId", @keydown.enter="getApplicationMessages()")
      button.btn.btn-outline-secondary(type="button", style="margin-left: 5px", @click="getApplicationMessages()") GET
    table.table.table-sm
      thead
        tr
          th(scope="col") Application ID
          th(scope="col") Payload
      tbody
        tr(v-for="m in messages")
          td {{ m.applicationId }}
          td {{ m.payload }}
</template>

<script>
import { connectApp } from 'radixdlt-js-lite'

export default {
  name: 'ListApplicationMessages',
  props: ['token'],
  data () {
    return {
      applicationId: '',
      messages: []
    }
  },
  methods: {
    getApplicationMessages () {
      connectApp(this.token)
        .then(radixConnection => {
          const messaging = radixConnection.getMessaging()

          this.messages = []

          // Get old and new application messages
          messaging.getApplicationMessages(this.applicationId).subscribe(message => {
            this.messages.push(message)
          })
        })
        .catch(error => {
          console.log(`Error while re-using a token: ${error}`)
        })
    }
  }
}
</script>
