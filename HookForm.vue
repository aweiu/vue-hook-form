<template>
  <form @submit='submit'>
    <slot></slot>
  </form>
</template>
<script>
  import formSerialize from 'form-serialize'
  var hookForm = {
    json: false,
    props: ['method', 'action', 'beforeSerialize', 'onSubmit', 'json', 'disabled'],
    methods: {
      submit (e) {
        if (e) e.preventDefault()
        if (typeof hookForm.onSubmit !== 'function' && typeof this.onSubmit !== 'function') return console.warn('please set me an \'onSubmit\' handler!')
        if (!this.disabled) {
          var next = () => {
            var request = {
              url: this.action,
              method: (this.method || 'post').toLowerCase(),
              body: formSerialize(this.$el, {hash: this.json !== undefined ? this.json : hookForm.json}),
              hookFormVm: this
            }
            this.runHook('onSubmit', request)
          }
          this.runHook('beforeSerialize', next)
        }
      },
      runHook (name, arg) {
        if (typeof this[name] === 'function') this[name](arg)
        else if (typeof hookForm[name] === 'function') hookForm[name](arg)
      }
    }
  }
  export default hookForm
</script>
