<template lang="pug">
  div
    h3 Send Transaction
    input.form-control(type="text", placeholder="Destination address..", v-model="destination")
    input.form-control(type="text", placeholder="Asset ISO..", v-model="asset", disabled)
    input.form-control(type="number", placeholder="Min: 0.00001", min="0.00001", step="0.00001", v-model="quantity")
    textarea.form-control(rows="4", placeholder="Optional message..", v-model="message")
    div(style="display: flex; align-items: center; justify-content: flex-end")
      button.btn.btn-outline-secondary(type="button", @click="sendTransaction()") SEND
</template>

<script>
import { connectApp } from 'radixdlt-js-lite'

export default {
  name: 'SendTransaction',
  props: ['token'],
  data () {
    return {
      destination: '',
      asset: 'TEST',
      quantity: 0.00001,
      message: ''
    }
  },
  methods: {
    sendTransaction () {
      connectApp(this.token)
        .then(radixConnection => {
          const wallet = radixConnection.getWallet()

          // Send transaction
          wallet.sendTransaction(this.destination, this.asset, this.quantity, this.message)
            .subscribe(
              response => console.log(`Your transaction was succesfully submitted: ${response}`),
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
