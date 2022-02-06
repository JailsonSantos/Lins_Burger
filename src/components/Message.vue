<template>
  <div class="message-container">
    <p>{{ msg }}</p>
  </div>
</template>

<script>
export default {
  name: "Message",
  props: {
    msg: String,
  },
};
</script>


<style scoped>
.message-container {
  color: #004085;
  background-color: #cce5ff;

  border: 2px solid #bbdaff;
  border-radius: 5px;

  padding: 10px;
  margin: 30px auto;

  max-width: 400px;
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>