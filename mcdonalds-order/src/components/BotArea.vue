<template>
  <div>
    <h2>ACTIVE Bot(s)</h2>
    <ul>
      <li v-for="bot in bots" :key="bot.id" :class="{ processing: bot.status === 'PROCESSING' }">
        Bot {{ bot.id }} - {{ bot.status }}
      </li>
    </ul>
    <h2>IDLE Bot(s)</h2>
    <ul>
      <li v-for="bot in idleBots" :key="bot.id">
        Bot {{ bot.id }} - IDLE
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  props: {
    bots: Array,
    idleBots: Array,
  },
};
</script>

<style scoped>
.processing {
  color: orange;
  font-weight: bold;
}
</style>