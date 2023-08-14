<template>
  <div>
    <h2>ACTIVE Bot(s)</h2>
    <ul>
      <li v-for="bot in activeBots" :key="bot.id" class="active-bots">
        Bot {{ bot.id }} - {{ bot.status }}
      </li>
    </ul>
    <h2>IDLE Bot(s)</h2>
    <ul>
      <li v-for="bot in idleBots" :key="bot.id" class="idle-bots">
        Bot {{ bot.id }} - IDLE
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  props: {
    activeBots: Array,
    idleBots: Array,
  },
};
</script>

<style scoped>
.active-bots {
  color: green;
  font-weight: bold;
}

.idle-bots {
  color: orange;
  font-weight: bold;
}
</style>