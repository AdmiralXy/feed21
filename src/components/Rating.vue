<template>
  <div>
    <div class="rating mb-3">
      <span v-for="index in 5" :key="index" class="pl-1">
        <b-icon :icon="index > value ? 'star' : 'star-fill'" variant="warning" font-scale="1.4" @click="setRating(index)"></b-icon>
      </span>
      <input type="text" value="5" hidden="">
      <span class="rating__total pl-1">{{ value }}</span>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Rating',
  props: {
    value: {
      type: Number,
      required: true
    }
  },
  methods: {
    setRating (value) {
      this.value = value
      this.$emit('input', value)
    },
  }
}
</script>

<style scoped>
.rating {
  height: 30px;
  color: #9b9b9b;
  font-size: 12px;
  line-height: 30px;
  user-select: none;
}

.rating__total {
  font-size: 14px;
  color: #5f88ff;
  line-height: 1.2;
}
</style>
