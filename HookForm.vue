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
              vm: this
            }
            this.runHook('onSubmit', request)
          }
          this.runHook('beforeSerialize', this, next)
        }
      },
      runHook (name, ...args) {
        if (typeof this[name] === 'function') this[name].apply(null, args)
        else if (typeof hookForm[name] === 'function') hookForm[name].apply(null, args)
      }
    }
  }
  export default hookForm
</script>
