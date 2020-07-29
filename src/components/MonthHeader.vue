<template>
    <div class="vc-row">
        <div v-for="weekDay in weekDays" :key="weekDay">{{weekDay}}</div>
    </div>
</template>

<script>
export default {
    name: 'MonthHeader',
    data() {
        return {
            weekDays: ['Mo', 'Tu', 'We', 'Th', 'Fr', 'Sa', 'Su']
        };
    }
}
</script>