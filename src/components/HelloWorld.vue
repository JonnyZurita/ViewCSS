<template>
  <div class="hello">
    <Button type="primary">Primary</Button>
    <Button type="secondary">Secondary</Button>
    <Button type="success" :round="'round'">Success</Button>
    <Button type="danger" outline>Danger</Button>
    <Button type="warning" :size="'mini'" html-type="submit" long>Warning</Button>
  </div>
</template>

<script>
import Button from './Button/'
export default {
  name: 'HelloWorld',
  components: {
    Button
  },
  data() {
    return {
      buttonSize: 'mini'
    }
  },
}
</script>
