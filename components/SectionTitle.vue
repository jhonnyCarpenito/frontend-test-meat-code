<template>
    <h2 class="section__title">
        {{ title }}
    </h2>
</template>

<script>
export default {
    props: {
        title: {
            type: String,
            required: true
        }
    },
}
</script>