<template>
    <div class="modal">
        <div class="modal-content">
            <span class="close-btn" v-on:click="onCloseRequest">&times;</span>
            <slot></slot>
        </div>
    </div>
</template>

<script>
export default {
    name: 'Modal',
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
    padding: 30px 2px 8px;
    width: 30%;
    background-color: white;
    border-radius: 8px;
    box-shadow: 0 24px 38px 3px rgba(0,0,0,0.14), 0 9px 46px 8px rgba(0,0,0,0.12), 0 11px 15px -7px rgba(0,0,0,0.2);
    position: relative;
}

/* The Close Button */
.close-btn {
    color: #aaa;
    font-size: 28px;
    display: inline-block;
    position: absolute;
    top: -2px;
    right: 2px;
}
  
.close-btn:hover,
.close-btn:focus {
    color: black;
    text-decoration: none;
    cursor: pointer;
}
</style>