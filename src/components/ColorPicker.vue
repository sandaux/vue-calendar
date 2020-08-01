<template>
    <div class="color-picker-items-container">
        <div v-for="color in availableColors" class="color-picker-item" v-bind:style="{backgroundColor: color}" v-bind:key="color" v-on:clicl="onColorSelected(color)">
            <span className="color-picker-ok" v-bind:style="{ display: selectedColor === color ? 'inline' : 'none'}">&#10004;</span>
        </div>
    </div>
</template>

<script>
export default {
    name: 'ColorPicker',
    props: {
        selectedColor: String
    },
    data() {
        return {
            availableColors: ['#8ED1FC', '#0693E3', '#ABB8C3', '#00D084', '#7BDCB5', '#FCB900', '#FF6900']
        }
    },
    methods: {
        onColorSelected(value) {
            this.$emit('color-selected', value);
        }
    }
}
</script>