<template>
  <div class="vc-week-row">
    <div class="vc-row vc-cell-container">
      <div
        v-for="weekDate in weekDates"
        v-bind:key="weekDate.valueOf()"
        class="vc-cell vc-day-cell"
        v-bind:class="{'vc-cell-off-range': weekDate.getMonth() !== month}"
        v-on:click="onDateCellClick(weekDate)"
      ></div>
    </div>

    <div class="vc-row">
        <div class="vc-cell vc-date-cell" v-for="weekDate in weekDates" v-bind:key="weekDate.valueOf()">
            <span class="vc-date-cell-header" v-bind:class="{'vc-date-cell-header-today': today.valueOf() === weekDate.valueOf()}">{{weekDate.getDate()}}</span>
        </div>
    </div>
    
    <div class="vc-row-content" v-for="(eventRow, index) in eventRows" v-bind:key="index">
        <EventBar v-for="eventBar in eventRow.eventBars" v-bind:id="eventBar.event.id" v-bind:title="eventBar.event.title" v-bind:color="eventBar.event.color" v-bind:dayStart="eventBar.dayStart" v-bind:dayEnd="eventBar.dayEnd" v-on:event-bar-click="onEventBarClick(eventBar.event)" v-bind:key="eventBar.event.id"></EventBar>
    </div>
  </div>
</template>

<script>
import EventBar from './EventBar.vue'
import { addDays, min, max, startOfDay } from 'date-fns'

function getWeekDates(start) {
    const weekDates = [];

    for (let i = 0; i < 7; i++) {
        weekDates.push(addDays(start, i));
    }

    return weekDates;
}

function getEventBars(weekStart, weekEnd, weekEvents) {
    const eventBars = [];

    for (let i = 0; i < weekEvents.length; i++) {
        const currentEvent = weekEvents[i];

        const dayStart = (max([weekStart, startOfDay(currentEvent.start)]).getDay() || 7) - 1;
        const dayEnd = (min([weekEnd, startOfDay(currentEvent.end)]).getDay() || 7) - 1;

        eventBars.push({ event: currentEvent, dayStart, dayEnd });
    }

    return eventBars;
}

function splitBetweenEventRows(eventBars) {
    const eventRows = [];

    for (let i = 0; i < eventBars.length; i++) {
        const eventBar = eventBars[i];

        const foundRow = eventRows.find(r => !r.eventBars.filter(b => b.dayStart <= eventBar.dayEnd && b.dayEnd >= eventBar.dayStart).length);
        if (!foundRow)
            eventRows.push({ eventBars: [eventBar] });
        else
            foundRow.eventBars.push(eventBar);
    }

    return eventRows;
}

export default {
    name: 'WeekRow',
    components: {
        EventBar
    },
    props: {
        start: Date,
        events: Array,
        month: Number
    },
    data() {    
        const weekDates = getWeekDates(this.start);

        return {
            weekDates,
            today: startOfDay(new Date())
        }
    },
    computed: {
        eventRows() {
            // todo: temp
            const weekEnd = this.weekDates[6];
            const weekEvents = this.events.filter(e => e.start <= weekEnd && e.end >= this.start);

            const eventBars = getEventBars(this.start, weekEnd, weekEvents);
            return splitBetweenEventRows(eventBars);
        }
    },
    methods: {
        onDateCellClick(weekDate) {
            this.$emit('date-cell-click', weekDate);
        },
        onEventBarClick(event) {
            this.$emit('event-bar-click', event);
        }
    }
}
</script>

<style scoped>
.vc-week-row {
    display: flex;
    flex-direction: column;
    position: relative;
    flex: 1 0;
}

.vc-cell-container {
    position: absolute;
    left: 0;
    top: 0;
    right: 0;
    bottom: 0;
}

.vc-cell-off-range {
    background-color: #e5e5e5;    
}

.vc-day-cell {
    border: 1px solid #DDD;
}

.vc-date-cell {
    text-align: center;
    z-index: 1;
}

.vc-row-content {
    display: flex;
    flex: 0.2 0;
}

.vc-date-cell-header {
    display: inline-block;
    margin: 3px;
    border-radius: 50%;
    height: 24px;
    width: 24px;    
}

.vc-date-cell-header-today {
    background-color: #1a73e8;
    color: #ffffff;
}
</style>