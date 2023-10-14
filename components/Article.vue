<template>
    <article class="article">
        <img :src="article.image" width="270" height="200" alt="Image Preview" />

        <div class="article__content">
            <h3 class="article__title">
                {{ article.title }}
            </h3>
            <p class="article__text">
                {{ article.content }}
            </p>

            <a href="#" class="article__button">ver más</a>
        </div>
    </article>
</template>
  
<script>
export default {
    props: {
        article: {
            type: Object,
            required: true
        }
    },
}
</script>