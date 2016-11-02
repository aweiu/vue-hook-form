<template>
  <form @submit='submit'>
    <slot></slot>
  </form>
</template>
<script>
  import formSerialize from 'form-serialize'
  var hookForm = {
    props: ['method', 'action', 'onSubmit', 'json', 'disabled', 'unhook'],
    methods: {
      submit (e) {
        if (e) e.preventDefault()
        if (typeof hookForm.onSubmit !== 'function' && typeof this.onSubmit !== 'function') return console.warn('please set me an \'onSubmit\' handler!')
        if (!this.disabled) {
          var request = {
            url: this.action,
            method: (this.method || 'post').toLowerCase(),
            body: formSerialize(this.$el, {hash: this.json !== undefined ? this.json : hookForm.json}),
            hookFormVm: this
          }
          if (!(this.unhook !== undefined) && typeof hookForm.onSubmit === 'function') hookForm.onSubmit(request)
          if (typeof this.onSubmit === 'function') this.onSubmit(request)
        }
      }
    }
  }
  export default hookForm
</script>
