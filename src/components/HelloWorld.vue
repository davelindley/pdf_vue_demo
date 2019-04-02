<template>
  <pdf :src="monkey" :page="2">
    <template slot="loading">
      loading content here...
    </template>
  </pdf>
</template>

<script>
import pdf from 'pdfvuer'
import monkey from '../assets/tracemonkey.pdf'
export default {

  components: {
      pdf
  },
    data: () => ({ monkey })
}
</script>