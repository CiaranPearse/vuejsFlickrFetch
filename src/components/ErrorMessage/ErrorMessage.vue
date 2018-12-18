<template>
	<v-alert
    :value="true"
    color="error"
    icon="warning"
    outline
  >
    {{ this.errorMsg }}
	</v-alert>
</template>

<script>
export default {
  props: ['errorMsg'],
  data () {
    return {
    }
  }
}
</script>
