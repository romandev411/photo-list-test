<template>
    <div class="filter-list">
        <ul class="filter-list__main"
            :style="{ 'min-height': `${heightItems}px` }"
        >
            <li v-for="itemList in array" class="filter-list__sect" :ref="'item'">
                <h3 class="filter-list__word" v-if="title === 0">{{itemList[0].title[0]}}</h3>
                <h3 class="filter-list__word" v-if="title === 1">{{itemList[0].albumId}}</h3>

                <div class="filter-list__wrap-item">
                    <list :array="itemList"></list>
                </div>
            </li>
        </ul>
    </div>
</template>

<script>
    import List from '@/components/list';

    export default {
        components: { List },
        props: {
            array: Array,
            column: Number,
            title: [String, Number],
        },
        data() {
            return {
                heightItems: 0,
                wordArray: [],
                isOpen: false,
            };
        },
        mounted() {
            this.heightWrap();
        },
        computed: {

        },
        methods: {
            heightWrap() {
                let array = this.$refs.item;

                if (!array) {
                    return;
                }

                let height = 0;

                array.forEach((e) => {
                    height = height + e.clientHeight;
                });

                this.heightItems = height / this.column;
            },
        },
        created() {

        },
        updated() {
            this.heightWrap();
        },
    }
</script>

<style lang="scss">
    * {
        box-sizing: inherit;
    }

    .filter-list {
        &__main {
            margin: 0;
            margin-left: -22px;
            margin-right: -22px;
            box-sizing: border-box;
            column-count: 4;
            column-gap: 0;
            counter-reset: item-counter;
            padding: 0;
            list-style-type: none;
            font-weight: 700;
        }

        &__sect {
            padding: 0 22px;
            margin-bottom: 26px;
            -webkit-column-break-inside: avoid;
            -moz-column-break-inside: avoid;
            break-inside: avoid;
        }

        &__word {
            width: 52px;
            text-align: center;
            margin: 0;
            text-transform: uppercase;

            &:not(:last-child) {
                margin-bottom: 8px;
            }
        }
    }
</style>