<template>
  <div class="container">
    <div>
      <!-- 通过href="javascript:void(0);"重置服务端渲染生成的href链接属性 -->
      <!-- <a class="client-render" href="javascript:void(0);" v-if="isClient || isMounted" @click="handleClick" data-test="client">xx client</a> -->
       <a class="client-render" v-if="isClient || isMounted" @click="handleClick" data-test="client">xx client</a>
      <a class="server-render" v-else href="https://m.baidu.com/index.php" data-test="server">xx server</a>
      <!-- <a class="server-render" @click="handleClickServer" v-else href="https://m.baidu.com/index.php" data-test="server">xx server</a> -->
    </div>
  </div>
</template>

<script>
import Logo from '~/components/Logo.vue'

export default {
  data() {
    return {
      isClient: __IS_CLIENT__,
      isMounted: false
    }
  },
  mounted() {
    this.isMounted = true
  },
  methods: {
    handleClick() {
      console.log('handleClick')
      const delay = 1000
      setTimeout(() => {
        console.log('handleClick setTimeout')
        window.location.href = 'https://m.baidu.com/index.php'
      }, delay);
      // 结果会导致在移动端[有可能]发生2次跳转到https://m.baidu.com/index.php
      // 如果页面在delay时间内没完成a链接href属性的跳转，就执行了window.location.href = 'https://m.baidu.com/index.php'的跳转，结果是只有delay之后的这次跳转
      // 如果在delay时间内已跳转新页面，在移动端Mi browser中，按返回键回到当前页时setTimeout回调会执行，导致发生二次跳转
    },
    handleClickServer() { // @click属性会被替换
      console.log('handleClickServer')
    }
  },
  components: {
    Logo
  }
}
</script>

<style>
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.title {
  font-family: 'Quicksand', 'Source Sans Pro', -apple-system, BlinkMacSystemFont,
    'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
</style>
