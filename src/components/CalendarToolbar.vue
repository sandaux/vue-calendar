<template>
    <div class="vc-toolbar">
        <button type="button" v-on:click="onCurrentMonthChange(currentMonth, -1)">Back</button>
        <button type="button" v-on:click="onCurrentMonthChange(new Date(), 0)">Today</button>
        <button type="button" v-on:click="onCurrentMonthChange(currentMonth, 1)">Next</button>
        <span class="vc-toolbar-label">{{currentMonth.toLocaleDateString('en', { year: 'numeric', month: 'long' })}}</span>
    </div>
</template>

<script>
import { addMonths } from "date-fns";

export default {
    name: "CalendarToolbar",
    props: {
    currentMonth: Date
  },
  methods: {
      onCurrentMonthChange(startDate, monthsToAdd) {
          this.$emit("current-month-change", addMonths(startDate, monthsToAdd));
    }
  }
};
</script>

<style scoped>
.vc-toolbar {
  font-size: 16px;
  display: flex;
  margin-bottom: 10px;
}

.vc-toolbar-label {
  flex-grow: 1;
}

.vc-toolbar button {
  background: none;
  text-transform: none;
  border: 1px solid #ccc;
  padding: 0.375rem 1rem;
}

.vc-toolbar button:active {
  background-image: none;
  box-shadow: inset 0 3px 5px rgba(0, 0, 0, 0.125);
  background-color: #e6e6e6;
  border-color: #adadad;
}

.vc-toolbar button:active:hover,
.vc-toolbar button:active:focus {
  color: #373a3c;
  background-color: #d4d4d4;
  border-color: #8c8c8c;
}

.vc-toolbar button:focus {
  color: #373a3c;
  background-color: #e6e6e6;
  border-color: #adadad;
}

.vc-toolbar button:hover {
  color: #373a3c;
  background-color: #e6e6e6;
  border-color: #adadad;
}
</style>