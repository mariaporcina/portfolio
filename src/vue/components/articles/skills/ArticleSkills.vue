<template>
    <Article class="article-skills"
             :model="model">
        <div class="article-content-wrapper">
            <div class="items-row-wrapper">
                <div class="row" :class="gutterClass">
                    <ArticleSkillsItem v-for="item in props.model.items"
                                       :item="item"
                                       :col-class="colClass"
                                       :max-items-per-line="Number(maxItemsPerLine)"
                                       :transparent-icon="props.model.getSetting('transparent_item_icon_background', false)"
                                       :small-icon="props.model.getSetting('small_icons', false)"/>
                </div>
            </div>
        </div>
    </Article>
</template>

<script setup>
import {computed} from "vue"
import Article from "/src/vue/components/articles/base/Article.vue"
import ArticleSkillsItem from "/src/vue/components/articles/skills/ArticleSkillsItem.vue"

const props = defineProps({
    /** @type {Article} **/
    model: {
        type: Object,
        required: true
    }
})

const maxItemsPerLine = computed(() => {
    const maxItemsPerLine = props.model.getSetting("max_items_per_line", 1)
    if(maxItemsPerLine < 0 || maxItemsPerLine > 3)
        throw new Error("[ArticleSkills] The setting max_items_per_line must be a number between 1 and 3.")
    return maxItemsPerLine
})

const gutterClass = computed(() => {
    return 'gx-5 gy-3 gy-md-4'
})

const colClass = computed(() => {
    switch (maxItemsPerLine.value) {
        default: return 'col-12'
        case 2: return 'col-12 col-sm-6 col-lg-12 col-xl-6'
        case 3: return 'col-12 col-sm-6 col-xxl-4'
    }
})
</script>

<style lang="scss" scoped>
@import "/src/scss/_theming.scss";

div.article-content-wrapper {
    display: flex;
    @include media-breakpoint-down(xl) {
        flex-direction: column;
    }
}

div.items-row-wrapper {
    display: flex;
    flex-grow: 1;
    width: 100%;
    align-items: center;

    .row {
        display: flex;
        flex-grow: 1;
    }
}
</style>
