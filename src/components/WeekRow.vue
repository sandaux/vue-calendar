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
            <span class="vc-date-cell-header" v-bind:class="{'vc-date-cell-header-today': today.valueOf() === weekDate.valueOf()}"></span>
        </div>
    </div>
    
    <div class="vc-row-content" v-for="(eventRow, index) in eventRows" v-bind:key="index">
        <EventBar v-for="eventBar in eventBars" id="eventBar.event.id" title="eventBar.event.title" color="eventBar.event.color" dayStart="eventBar.dayStart" dayEnd="eventBar.dayEnd" v-on:click="onEventClick(eventBar.event)" v-bind:key="eventBar.event.id"></EventBar>)}        
    </div>
  </div>
</template>

<script>
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
    props: {
        start: Date,
        events: [],
        month: Number
    },
    data() {
        const weekDates = getWeekDates(this.start);
        const eventBars = getEventBars(this.start, weekDates[6], this.events);
        const eventRows = splitBetweenEventRows(eventBars);

        return {
            weekDates,
            today: startOfDay(new Date()),
            eventRows
        }
    },
    methods: {
        onDateCellClick(weekDate) {
            this.$emit('on-date-cell-click', weekDate);
        },
        onEventClick(event) {
            this.$emit('on-event-click', event);
        }
    }
}
</script>