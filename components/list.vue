<template>
    <ul class="list">
        <li v-for="item in array"
            class="list__list-item"
            :title="item.title"
        >
            <div class="list__item">
                <div class="list__img-wrap">
                    <img class="list__img"
                         :src="item.thumbnailUrl"
                         :alt="item.title"
                    >
                </div>

                <div class="list__text">
                    {{item.title}}
                </div>
            </div>
        </li>
    </ul>
</template>

<script>
    export default {
        props: {
            array: Array,
        },
    }
</script>

<style lang="scss">
    .list {
        padding: 0;
        margin: 0;
        list-style-type: none;

        &__list-item {
            margin-bottom: 8px;
        }

        &__item {
            display: flex;
            align-items: center;
            height: 40px;
            border-radius: 8px;
            padding: 0 4px;
            background: #F7F8F9;
        }

        &__img-wrap {
            flex-shrink: 0;
            width: 32px;
            height: 32px;
            margin-right: 11px;
        }

        &__img {
            width: 100%;
            height: 100%;
            border-radius: 8px;
            object-fit: cover;
        }

        &__text {
            flex-grow: 1;
            overflow: hidden;
            text-overflow: ellipsis;
            white-space: nowrap;

            &:first-letter {
                text-transform: uppercase;
            }
        }
    }
</style>