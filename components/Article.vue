<template>
    <article class="article">
        <img :src="article.image" width="270" height="200" alt="Image Preview" />

        <div class="article_content">
            <p class="article_title">
                {{ article.title }}
            </p>
            <p class="article_text">
                {{ article.content }}
            </p>

            <a href="#" class="article_button">ver más</a>
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