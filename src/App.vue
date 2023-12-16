<script setup lang="ts">
import { ref } from "vue";
import "./style.css";

import useDate from "./composables/useDate";

const daysToAdd = ref(0);
const monthsToAdd = ref(0);
const heading = ref("Current Date");

const { date, getDay, getMonth, getYear, addDay, addMonth } = useDate();
const dates = ref(date.toString());

const handleInputChange = (event: Event) => {
  let { name, value } = event.target as HTMLInputElement;
  if (name === "day") {
    if (!value) value = "0";
    daysToAdd.value = parseInt(value);
  } else if (name === "month") {
    if (!value) value = "0";
    monthsToAdd.value = parseInt(value);
  }
};

const handleAddDaysAndMonths = () => {
  const newDate = addMonth(monthsToAdd.value, addDay(daysToAdd.value, date));
  return newDate;
};

const handleClick = () => {
  heading.value = "Updated date:";
  dates.value = handleAddDaysAndMonths().toString();
};
</script>

<template>
  <div>
    <h2 class="header">Today</h2>
    {{ getDay() }}, {{ getMonth() }} {{ getYear() }}.
    <br />
    <h2>{{ heading }}</h2>
    <p>Date: {{ dates }}</p>
    <br />
    <div class="input-container">
      Add Day:
      <input
        name="day"
        type="number"
        @change="handleInputChange"
        class="input"
      />
      <br />
      Add Month:
      <input
        name="month"
        type="number"
        @change="handleInputChange"
        class="input"
      />
      <br />
    </div>
    <button @click="handleClick" class="btn">Click to update</button>
  </div>
</template>

<style scoped>
.header {
  color: #646cff;
}
.input-container {
  padding: 30px;
  border: 0.5px solid #000;
}
.btn {
  border: none;
  font-weight: 500;
  margin-top: 20px;
  color: #f9f9f9;
  padding: 8px 25px;
  border-radius: 6px;
  background-color: #747bff;
}

.input {
  margin-bottom: 20px;
  padding: 8px 25px;
  border-radius: 4px;
  border: 0.5px solid #000;
}
</style>
