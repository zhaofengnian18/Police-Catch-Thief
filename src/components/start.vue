<template>
  <div class="start">
    <h1>Police Catch Thief</h1>
    <div class="select">
      <span>difficulty：</span>
      <el-select v-model="difficulty" placeholder="请选择">
        <el-option
          v-for="item in options"
          :key="item.value"
          :label="item.label"
          :value="item.value"
        ></el-option>
      </el-select>
    </div>
    <el-button @click="$emit('startgame',difficulty)" type="primary">start</el-button>
  </div>
</template>

<script>
export default {
  name: "Start",
  data() {
    return {
      options: [
        {
          value: "easy",
          label: "easy"
        },
        {
          value: "middle",
          label: "normal"
        },
        {
          value: "difficult",
          label: "difficult"
        },
      ],
      difficulty: "easy"
    };
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .select {
    margin-bottom: 20px;
  }
</style>