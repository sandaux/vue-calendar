<template>
    <div class="vc-calendar">
        <CalendarToolbar v-bind:currentMonth="currentMonth" v-on:current-month-change="handleCurrentMonthChange" />
        <MonthHeader />
        <WeekRow v-for="weekRowData in weekRowsData" v-bind:start="weekRowData.start" v-bind:events="weekRowData.events" v-bind:month="weekRowData.month" v-bind:key="weekRowData.start.valueOf()" v-on:date-cell-click="handleDateCellClick" v-on:event-bar-click="handleEventBarClick" />
        <Modal v-if="eventToEdit" v-on:close-request="handleModalCloseRequest">
            <EventDetails v-if="eventToEdit" v-bind:event="eventToEdit" v-on:event-change="handleEventToEditChange" v-on:event-save-request="handleEventToEditSaveRequest"></EventDetails>
        </Modal>
    </div>
</template>

<script>
import MonthHeader from './MonthHeader.vue'
import WeekRow from './WeekRow.vue'
import CalendarToolbar from './CalendarToolbar.vue'
import Modal from './Modal.vue'
import EventDetails from './EventDetails.vue'
import { addDays, addWeeks, startOfMonth, endOfMonth, endOfDay } from 'date-fns';

const defaultColor = '#8ED1FC';

function calculateFirstCellDate(start) {
    let monthStart = startOfMonth(start);

    return addDays(monthStart, 1 - monthStart.getDay());
}

export default {
    name: 'Calendar',
    components: {
        CalendarToolbar, MonthHeader, WeekRow, Modal, EventDetails
    },
    data() {
        return {
            currentMonth: startOfMonth(new Date()),
            events: [
                { id: '1', title: 'do smth 1', start: addDays(new Date(), -20), end: addDays(new Date(), -10), color: defaultColor },
                { id: '2', title: 'do smth 2', start: addDays(new Date(), -1), end: addDays(new Date(), 1), color: defaultColor },
                { id: '3', title: 'do smth 3', start: addDays(new Date(), 1), end: addDays(new Date(), 19), color: defaultColor },
            ],
            eventToEdit: undefined
        }
    },
    computed: {
        weekRowsData() {
            const month = this.currentMonth.getMonth();
            const weekRowsData = [];

            let currentDate = calculateFirstCellDate(this.currentMonth);
            const endOfCurrentMonth = endOfMonth(this.currentMonth);

            while (currentDate <= endOfCurrentMonth) {
                weekRowsData.push({ start: currentDate, events: this.events, month });
                currentDate = addWeeks(currentDate, 1);
            }

            return weekRowsData;
        }
    },
    methods: {
        handleCurrentMonthChange(month) {
            this.currentMonth = month;
        },
        handleEventBarClick(event) {
            this.eventToEdit = event;
        },
        handleDateCellClick(cellDate) {
            this.eventToEdit = {
                title: 'New event',
                start: cellDate,
                end: endOfDay(cellDate),
                color: defaultColor
            };

            this.events.push({...this.eventToEdit});            
        },
        handleEventToEditChange(changedEvent) {
            this.events = this.events.filter(event => event.id !== changedEvent.id).concat(changedEvent);
        },
        handleEventToEditSaveRequest(eventToSave) {
            const calendarEvents = this.events.filter(event => event.id !== eventToSave.id).concat(eventToSave);

            // todo: temp solution
            if (!eventToSave.id) {
                eventToSave.id = Math.random().toString(36).substring(7);
            }

            this.eventToEdit = undefined;
            this.events = calendarEvents;
        },
        handleModalCloseRequest() {
            const calendarEvents = this.events.filter(event => event.id !== this.eventToEdit.id);

            if (this.eventToEdit?.id) {
                calendarEvents.push(this.eventToEdit);
            }

            this.eventToEdit = undefined;
            this.events = calendarEvents;
        }
    }
}
</script>

<style>
.vc-calendar {
    display: flex;
    flex-direction: column;
    min-height: calc(100vh - 100px)
}

.vc-row {
    display: flex;
    flex-direction: row;
}

.vc-cell {
    flex: 1 0;
}

.form-group {
    display: flex;
    padding: 4px;
}

.form-input {
    flex: 1 0;
}

.btn {
    user-select: none;
    border: 1px solid transparent;
    border-radius: .25rem;
    padding: .375rem .75rem;
}

.btn:hover {
    background-color: #e2e6ea;
    border-color: #dae0e5;
}
</style>