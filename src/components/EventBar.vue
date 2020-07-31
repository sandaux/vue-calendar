<template>
    <div class="vc-event-bar" v-bind:class="{'rc-event-bar-temp': id}" v-on:click="onEventBarClick" v-bind:style="eventBarStyle">
        <span>{{title}}</span>
    </div>
</template>

<script>
const dayWidth = 100 / 7;

export default {
    name: 'EventBar',
    props: {
        id: String,
        title: String,
        color: String,
        dayStart: Number,
        dayEnd: Number
    },
    computed: {
        eventBarStyle() {
            const left = this.dayStart * dayWidth;
            const width = (this.dayEnd - this.dayStart + 1) * dayWidth;            

            return {
                left: `${left}%`,
                width: `${width}%`,
                backgroundColor: this.color
            }
        }
    },
    methods: {
        onEventBarClick() {
            this.$emit('on-event-bar-click');
        }
    }
}
</script>

<style>
.vc-event-bar {
    position: absolute;
    border-radius: 4px;
}

.vc-event-bar-temp {
    opacity: 0.5;
    box-shadow: 0 6px 10px 0 rgba(0,0,0,0.14), 0 1px 18px 0 rgba(0,0,0,0.12), 0 3px 5px -1px rgba(0,0,0,0.2);
}
</style>