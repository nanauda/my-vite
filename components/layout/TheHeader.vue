<template>
  <header>
    <h1>{{ title }}</h1>
    <button @click="toLogin">ログイン</button>
  </header>
</template>

<script>
export default {
  props: {
    title: {
      type: String,
      default: 'default'
    }
  },
  setup(props, context) {
    function toLogin(params) {
      console.log('toLogin')
    }
    // NG: {...props} リアクティブ性がなくなる
    console.log(props, {...context})
    return {
      toLogin
    }
  }
}
</script>