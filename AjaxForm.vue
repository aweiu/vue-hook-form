<template>
  <form @submit='submit'>
    <slot></slot>
  </form>
</template>
<script>
  import formSerialize from 'form-serialize'
  var ajaxForm = {
    props: ['method', 'action', 'onSubmit', 'json', 'disabled', 'unhook'],
    methods: {
      submit (e) {
        if (e) e.preventDefault()
        if (typeof ajaxForm.onSubmit !== 'function' && typeof this.onSubmit !== 'function') return console.warn('please set me an \'onSubmit\' handler!')
        if (!this.disabled) {
          var request = {
            url: this.action,
            method: (this.method || 'post').toLowerCase(),
            body: formSerialize(this.$el, {hash: this.json !== undefined ? this.json : ajaxForm.json}),
            ajaxFormVm: this
          }
          if (!(this.unhook !== undefined) && typeof ajaxForm.onSubmit === 'function') ajaxForm.onSubmit(request)
          if (typeof this.onSubmit === 'function') this.onSubmit(request)
        }
      }
    }
  }
  export default ajaxForm
</script>
