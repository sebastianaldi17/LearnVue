<script>
import LearnComponent from './components/LearnComponent.vue'
export default {
  components: {
    LearnComponent
  },
  data() {
    return {
      emitReciever: "No emitted event yet"
    }
  }
}
</script>

<template>
  <LearnComponent passed-val="I'm a passed prop" @emit-event="(emitResponse) => { emitReciever = emitResponse }">
    This is content inside slot element
  </LearnComponent>
  <p>{{ emitReciever }}</p>
</template>