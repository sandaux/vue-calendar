<template>
    <form v-on:submit.prevent="onSave">
        <div class="form-group">
            <input class="form-input" name="title" v-model="title" required />
        </div>
        <div class="form-group">
            <input class="form-input" name="start" type="date" v-bind:value="format(start, 'yyyy-MM-dd')" v-on:input="start = $event.target.valueAsDate" required />
        </div>
        <div class="form-group">
            <input class="form-input" name="end" type="date" v-bind:value="format(end, 'yyyy-MM-dd')" v-on:input="end = $event.target.valueAsDate" required />
        </div>
        <div class="form-group">
            <ColorPicker v-bind:selected-color="color" v-on:color-selected="onColorSelected" />
        </div>
        <button class="btn" type="submit">Save</button>
    </form>
</template>

<script>
import { format } from 'date-fns'
import ColorPicker from './ColorPicker.vue'

export default {
    name: 'EventDetails',
    components: {
        ColorPicker
    },
    props: {
        event: Object        
    },
    data() {
        return {
            title: this.event.title,
            color: this.event.color,
            start: this.event.start,
            end: this.event.end
        }
    },
    computed: {        
        combinded() {
            return `${this.title}|${this.color}|${this.start}|${this.end}`;
        }
    },
    watch: {
        combinded() {
            this.onChange();
        }
    },
    methods: {
        format,
        onColorSelected(value) {
            this.color = value;
        },
        onChange() {
            this.$emit("event-change", {
                id: this.event.id,
                title: this.title,
                color: this.color,
                start: this.start,
                end: this.end,
            });            
        },
        onSave() {
            this.$emit("event-save-request", {
                id: this.event.id,
                title: this.title,
                color: this.color,
                start: this.start,
                end: this.end,
            });  
        }
    }
}
</script>