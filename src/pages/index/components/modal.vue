<template>
  <div class="modal" >
    <div class="box">
      <div class="content">
        <h2 class="title">{{title}}</h2>
        <div class="text">{{content}}</div>
      </div>
      <div class="btn">
        <div class="left" @click="cancel">{{cancelText}}</div>
        <button lang='zh_CN' open-type="getPhoneNumber" class="right" @getphonenumber="callback" >{{confirmText}}</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    title: {
      type: String,
      default: '提示'
    },
    content: {
      type: String,
      default: ''
    },
    cancelText: {
      type: String,
      default: '放弃登录'
    },
    confirmText: {
      type: String,
      default: '确认授权'
    }
  },
  methods: {
    callback (e) {
      this.$emit('confirm', e)
    },
    cancel (evt) {
      this.$emit('close', evt)
    }
  }
}
</script>

<style scoped>

</style>
