<template>
    <header class="bg-blue-300 p-4">
        <p class="text-2xl font-bold"> {{ text }}</p>
    </header>
</template>

<script>

export default {
    name: "Header",
    props: {
        text: {
            type: String,
            required: true,
        },
    }
}
</script>
