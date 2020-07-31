<template>
    <div class="vc-row">
        <div class="vc-cell" v-for="weekDay in weekDays" v-bind:key="weekDay">{{weekDay}}</div>
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