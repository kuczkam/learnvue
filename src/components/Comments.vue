<template>
<div class="comment-wrapper">
    <div class="comments" v-for="(comment, index) in commentsList" :key="index">
        <h1>Komentarz tego gówna</h1>
        <p>{{ comment.body }}</p>
    </div>
    <slot></slot>
</div>
</template>

<script>
export default {
    props: [
        'commentsList'
    ]
}
</script>

<style scoped>
h1 {
    font-size: 20px;
}
</style>