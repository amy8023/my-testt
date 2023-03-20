<template>
  <Standalone />
  <ETH />
</template>

<script>
import Standalone from './components/standalone.vue'
import Html from './components/html.vue'
import ETH from './components/eth.vue'

export default {
  name: 'App',
  components: {
    Standalone,
    ETH
  }
}
</script>
