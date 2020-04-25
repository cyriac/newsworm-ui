<template>
  <div>
    <b-navbar variant="light" type="dark" class="pt-2 pb-1 mb-3" sticky>
      <b-navbar-brand to="/">
        <logo />
      </b-navbar-brand>
    </b-navbar>
    <nuxt />
  </div>
</template>

<script>
import Logo from '~/components/Logo.vue'

export default {
  components: {
    Logo
  }
}
</script>
