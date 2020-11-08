<template>
    <div class="wrap">
        <vendor-filter @increment="count = $event" :count="count">
            <template #gallery>
                <filter-list :array="fullGalleryArray"
                             :column="4"
                             v-if="count === 0"
                             :title="count"
                >
                </filter-list>

                <filter-list :array="albumArray"
                             :column="4"
                             v-if="count === 1"
                             :title="count"
                >
                </filter-list>
            </template>
        </vendor-filter>

        <div class="wrap__bg"></div>
    </div>
</template>

<script>
import VendorFilter from '@/components/vendor-filter';
import FilterList from '@/components/filter-list';
import List from '@/components/list';

const API_URL = 'http://jsonplaceholder.typicode.com/photos';

export default {
    components: { VendorFilter, FilterList, List },

    data () {
        return {
            rawGallery: [],
            fullGalleryArray: [],
            albumArray: [],
            count: 0,
        };
    },

    async mounted() {
        await this.fetchGalleryList();
        this.galleryList();
        this.albumList();
    },

    computed: {

    },

    created () {

    },

    methods: {
        async fetchGalleryList() {
            await fetch(API_URL)
                .then(response => response.json())
                .then((result) => {
                    this.rawGallery = result;
                });
        },
        galleryList() {
            if (!this.rawGallery) {
                return [];
            }

            let map = ((m, a) => (a.forEach(s => {
                let a = m.get(s.title[0]) || [];

                m.set(s.title[0], (a.push(s), a));
            }), m))(new Map(), this.rawGallery);

            let arr = Array.from(map).sort((a, b) => {
                return a[0].charCodeAt(0) - b[0].charCodeAt(0);
            });

            this.fullGalleryArray = arr.map( (item) => {
                return item[1].splice(0,10)
            });
        },
        albumList() {
            if (!this.rawGallery) {
                return [];
            }

            let map = ((m, a) => (a.forEach(s => {
                let a = m.get(s.albumId) || [];

                m.set(s.albumId, (a.push(s), a));
            }), m))(new Map(), this.rawGallery);

            let arr = Array.from(map).map((e) => {
                return e[1].splice(0, 10);
            }).splice(0 ,30);

            this.albumArray = arr.map(function (e) {
                e.sort(function (a, b) {
                    return a.title.charCodeAt(0) - b.title.charCodeAt(0);
                })
            });

            this.albumArray = arr;
        },
    },
};
</script>

<style lang="scss">
.wrap {
    position: relative;
    height: 100vh;
    display: flex;

    &__bg {
        position: fixed;
        top: 0;
        right: 0;
        bottom: 0;
        left: 0;
        z-index: -1;
        background: rgba(27, 32, 79, 0.2);
    }
    
    &__album-filter-title {
        width: 52px;
        text-align: center;
    }
}
</style>
