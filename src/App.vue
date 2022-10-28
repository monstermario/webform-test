<template>
  <router-view />
</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
.input-area,
.form-radio {
  width: 100%;
  display: flex;
}
.form-input {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  margin-bottom: 20px;
}
.form-input input[type="text"],
.form-input .select-text,
.input-multi {
  width: 100%;
  border-radius: 8px;
  border: 1px solid #aaa;
  font-size: 14px;
  line-height: 16px;
  padding: 5px 15px;
  min-height: 28px;
}
.form-input > label {
  margin-bottom: 5px;
  font-weight: bold;
}
.input-required-mark {
  color: red;
}
.input-error {
  font-size: 12px;
  line-height: 14px;
  min-height: 14px;
  color: red;
  padding: 0 10px;
  margin-top: 5px;
}
ul li {
  list-style: none;
}
</style>
