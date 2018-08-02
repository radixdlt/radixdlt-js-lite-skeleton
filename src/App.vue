<template lang="pug">
  div#app
    img.logo(style="height: 80px; float: right", src="./assets/logo.png")
    div.site-wrapper-inner
      h1 radixdlt-js-lite
      h4 skeleton example

      //- User's Address & Balance
      div.row
        div.col-md-12.col-lg-9.column
          Address(:address="address")
        div.col-md-12.col-lg-3.column
          Balance(:balance="balance")

      //- Tabs
      vue-tabs

        //- 1st Tab: Transactions
        v-tab(title="Transactions")
          div.row
            div.col-md-12.column
              SendTransaction(:token="token")
              ListTransactions(:transactions="transactions")

        //- 2nd Tab: Messages
        v-tab(title="Messages")
          div.row
            div.col-md-12.column
              SendMessage(:token="token")
              ListMessages(:messages="messages")
              
        //- 3rd Tab: Application Messages
        v-tab(title="Application Messages")
          div.row
            div.col-md-12.column
              SendApplicationMessage(:token="token")
              ListApplicationMessages(:token="token")
</template>

<script>
import Address from '@/components/Address'
import Balance from '@/components/Balance'

import SendTransaction from '@/components/transactions/SendTransaction'
import ListTransactions from '@/components/transactions/ListTransactions'

import SendMessage from '@/components/messages/SendMessage'
import ListMessages from '@/components/messages/ListMessages'

import SendApplicationMessage from '@/components/application-messages/SendApplicationMessage'
import ListApplicationMessages from '@/components/application-messages/ListApplicationMessages'

import { registerApp } from 'radixdlt-js-lite'

export default {
  name: 'App',
  // List of imported components, to be used on this component
  components: {
    Address,
    Balance,
    SendTransaction,
    SendMessage,
    SendApplicationMessage,
    ListTransactions,
    ListMessages,
    ListApplicationMessages
  },
  data () {
    return {
      address: 'address',
      balance: 'balance',
      transactions: [],
      messages: [],
      applicationMessages: [],
      token: null
    }
  },
  created () {
    const name = 'radixdlt-js-lite-skeleton'
    const description = 'Minimal sample App to wrap the radixdlt-js-lite library functionality.'
    // The list of permissions requested by this app
    const permissions = [
      'address',
      'balance',
      'send_transactions',
      'send_messages',
      'send_application_messages',
      'transactions',
      'messages',
      'application_messages'
    ]

    registerApp(name, description, permissions)
      .then(radixConnection => {
        // Save the token to be pass as a parameter to each sub-component
        this.token = radixConnection.token

        // Get an instance of the Wallet module
        const wallet = radixConnection.getWallet()
        // Get an instance of the Messaging module
        const messaging = radixConnection.getMessaging()

        // Get address
        radixConnection.getAddress().subscribe(address => { this.address = address })
        // Get balance
        wallet.getBalance().subscribe(balance => { this.balance = balance })
        // Get transactions
        wallet.getTransactions().subscribe(transaction => { this.transactions.push(transaction) })
        // Get messages
        messaging.getMessages().subscribe(message => { this.messages.push(message) })
        // Get application messages
        messaging.getApplicationMessages('test-app-id').subscribe(message => { this.applicationMessages.push(message) })
      })
      .catch(error => {
        console.log(`Error while registering your app: ${JSON.stringify(error)}`)
      })
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 30px;
}
.logo {
  height: 50px;
}

html,
body {
  height: 100%;
  padding: 0 30px;
}
body {
  color: #fff;
}

.btn:focus, .btn-custom:focus{
  outline: 0 !important;
}
.btn-custom {
  border: none;
  font-weight: bold;
}
.btn-custom:hover,
.btn-custom:focus,
.btn-custom:active,
.btn-custom.active {
  color: #ffffff;
  background-color: #7e62ad;
  border: none;
}

.form-control::-webkit-input-placeholder { /* Chrome */
  color: #e6d9d9;
}
.form-control:-ms-input-placeholder { /* IE 10+ */
  color: #e6d9d9;
}
.form-control::-moz-placeholder { /* Firefox 19+ */
  color: #e6d9d9;
  opacity: 1;
}
.form-control:-moz-placeholder { /* Firefox 4 - 18 */
  color: #e6d9d9;
  opacity: 1;
}

.column {
  padding: 10px 15px;
}
.column input, .column textarea, .column .form-check {
  margin-bottom: 5px;
}

.list {
  margin-top: 10px;
}
</style>
