<template>
    <div class="modal">
        <div class="modal-content">
            <div class="buttons-panel">
                <span v-for="panelButton in panelButtons" v-bind:class="panelButton.class" v-bind:key="panelButton.name" v-on:click="panelButton.onClick" v-html="panelButton.unicodeChar"></span>
            </div> 
            <slot></slot>
        </div>
    </div>
</template>

<script>
export default {    
    name: 'Modal',
    props: {
        actionButtons: Array
    },
    data() {
        const closeButton = {
            name: 'close',
            class: 'action-btn action-btn-close',
            onClick: this.onCloseRequest,
            unicodeChar: '&times;',
            showIf: () => true
        };

        return {
            panelButtons: (this.actionButtons || []).concat(closeButton).filter(ab => ab.showIf()).reverse()
        }
    },
    methods: {
        onCloseRequest() {
            this.$emit('close-request');
        }
    }
}
</script>

<style scoped>
.modal {
    display: block;
    position: fixed;
    z-index: 100;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
}

.modal-content {
    margin: auto;
    padding: 2px 2px 8px;
    width: 30%;
    background-color: white;
    border-radius: 8px;
    box-shadow: 0 24px 38px 3px rgba(0,0,0,0.14), 0 9px 46px 8px rgba(0,0,0,0.12), 0 11px 15px -7px rgba(0,0,0,0.2);
    position: relative;
}

.buttons-panel {
    display: flex;
    flex-direction: row-reverse;
}

.action-btn {
    color: #aaa;
    font-size: 28px;
}

.action-btn-close {
    font-size: 34px;
}

.action-btn:hover,
.action-btn:focus {
    color: black;
    text-decoration: none;
    cursor: pointer;
}
</style>