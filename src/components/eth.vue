<template>
  ETH:
  <button @click="connect" :disabled="disabled">{{ msg }}</button>
  <br />
</template>

<script>
import { EthereumProvider } from '@walletconnect/ethereum-provider'

export default {
  name: 'HelloWorld',
  data() {
    return {
      projectId: '3f5b78ae39a9eb6fa5d9b40570489df8',
      namespaces: {
        eip155: {
          methods: [
            'eth_sendTransaction',
            'eth_signTransaction',
            'eth_sign',
            'personal_sign',
            'eth_signTypedData'
          ],
          chains: ['eip155:1', 'eip155:56', 'eip155:66'],
          events: ['accountsChanged', 'chainChanged'],
          rpcMap: {
            'eip155:1': `https://main.infura.io/v3/`
          }
        }
      },
      disabled: false,
      msg: 'connect',
      signClient: undefined
    }
  },
  mounted() {
  },
  methods: {
    async connect() {
      const provider = await EthereumProvider.init({
        projectId: '3f5b78ae39a9eb6fa5d9b40570489df8', // REQUIRED your projectId
        chains: ['eip155:1', 'eip155:56', 'eip155:66'], // REQUIRED chain ids
        methods: [
          'eth_sendTransaction',
          'eth_signTransaction',
          'eth_sign',
          'personal_sign',
          'eth_signTypedData'
        ], // OPTIONAL ethereum methods
        events: ['accountsChanged', 'chainChanged'], // OPTIONAL ethereum events
        rpcMap: [`https://main.infura.io/v3/`], // OPTIONAL rpc urls for each chain
        metadata: {
          name: 'Ave.ai',
          description: '如果你是一个传说',
          url: 'https://ave.ai/',
          icons: ['https://fastly.jsdelivr.net/npm/@vant/assets/cat.jpeg']
        }, // OPTIONAL metadata of your app
        showQrModal: true // OPTIONAL - `true` by default
        // qrModalOptions, // OPTIONAL - `undefined` by default, see https://docs.walletconnect.com/2.0/web3modal/theming
      })
      console.log('1111', provider)
      await provider.connect(this.namespaces.eip155.rpcMap)
      await provider.enable()
    }
  }
}
</script>
