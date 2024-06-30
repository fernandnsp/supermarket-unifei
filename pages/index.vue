<template>
  <div>
    <Header />
    <Buy />
  </div>
</template>

<script>
import Header from '~/components/Header'
import Buy from '~/components/Buy'

export default {
  name: 'IndexPage',

  components:{
    Header,
    Buy,
  }
}
</script>
