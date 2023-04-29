<template>
    <div v-show="show">
        <p>Block 3</p>
    </div>
</template>

<script lang="ts">
import { computed, reactive, ref, watch } from 'vue';

export default {
    name: "TestComponent",
    data() {
        return {
            Text: "",
            show: true
        }
    },

    props: {
        text: String
    },
    setup(props) {
        console.log(props.text)
        
        return {}
    },

}
</script>

<style scoped></style>