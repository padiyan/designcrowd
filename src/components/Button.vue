<template>
    <button
        :id="id"
        @click="clickHandler"
        class=" bg-blue-500 text-white font-bold py-2 px-4 rounded"
        :class="disabled ? 'cursor-not-allowed opacity-50' : 'hover:bg-blue-700'">
        {{ name }}
    </button>
</template>

<script>

export default {
    name: "Button",
    props: {
    id: {
        type: String 
    },
    className: {
        type: String
    },
    name: {
        type: String,
        required: true
    },
    clickHandler: {
        type: Function,
        required: true
    },
    disabled: {
        type: Boolean
    }
  }
}
</script>
