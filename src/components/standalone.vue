<template>
  Standalone:
  <button @click="connect" :disabled="disabled">{{ msg }}</button>
  <br />
</template>

<script>
import { SignClient } from '@walletconnect/sign-client'
import { Web3Modal } from '@web3modal/standalone'

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
          events: ['accountsChanged', 'chainChanged']
        }
      },
      disabled: false,
      msg: 'init...',
      signClient: undefined
    }
  },
  mounted() {
    this.initialize()
  },
  methods: {
    async initialize() {
      try {
        this.disabled = true
        this.signClient = await SignClient.init({
          projectId: this.projectId,
          metadata: {
            name: 'Ave.ai',
            description: '如果你是一个传说',
            url: 'https://ave.ai/',
            icons: ['https://fastly.jsdelivr.net/npm/@vant/assets/cat.jpeg']
          }
        })
        console.log('11111111', this.signClient)
        this.disabled = false
        this.msg = 'Connect Wallet'
      } catch (err) {
        console.error(err)
      }
    },
    async connect() {
      try {
        if (this.signClient) {
          const web3Modal = new Web3Modal({
            projectId: this.projectId,
            standaloneChains: this.namespaces.eip155.chains
          })
          const { uri, approval } = await this.signClient.connect({
            requiredNamespaces: this.namespaces
          })
          if (uri) {
            await web3Modal.openModal({ uri })
            await approval()
            web3Modal.closeModal()
          }
        }
      } catch (err) {
        console.error(err)
      }
    }
  }
}
</script>
