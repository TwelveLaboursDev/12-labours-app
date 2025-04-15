<template>
    <div class="page-outer">
        <breadcrumb-trail :breadcrumb="breadcrumb" :title="pageTitle" />
        <div class="detail-container">
            <div class="vertical-flex">
                <div class="publication-title">
                    <h1>Publications</h1>
                    <tab-nav :tabs="tabs" :active-tab="activeTab" @tabClick="onTabClick($event)" />
                </div>
                <div class="publication-detail">
                    <div v-html="publication.pContent.html"></div>
                    <div v-if="publication.caContent">
                        <h1>Conference abstracts</h1>
                        <div v-html="publication.caContent.html"></div>
                    </div>
                </div>
            </div>
            <div class="back-to">
                <nuxt-link to="/about">&lt; Back to About</nuxt-link>
            </div>
        </div>
    </div>
</template>

<script>
import graphcmsQuery from '@/services/graphcmsQuery'

export default {
    name: "ProjectPage",

    async asyncData({ $graphcms }) {
        const publications = await graphcmsQuery.publicationsItem($graphcms);
        const years = publications.values.map((pub) => {
            return { label: pub.year, name: pub.year }
        })
        return { publicationsList: publications.values, tabs: years };
    },

    data: () => {
        return {
            pageTitle: 'Publication',
            breadcrumb: [
                {
                    to: {
                        name: 'index'
                    },
                    label: 'Home'
                },
                {
                    to: {
                        name: 'about'
                    },
                    label: 'About'
                }
            ],
            activeTab: undefined
        };
    },

    computed: {
        publication() {
            return this.publicationsList.filter((pub) => pub.year === this.activeTab)[0]
        }
    },

    methods: {
        onTabClick(tab) {
            this.activeTab = tab
        }
    },

    created() {
        this.activeTab = this.tabs[0].name;
    },
};
</script>

<style scoped lang="scss">
.detail-container {
    padding: 2rem 20%;

    @media only screen and (max-width: $viewport-sm) {
        padding: 2rem 4%;
    }

    @media only screen and (min-width: $viewport-lg) {
        padding: 2rem 19.38rem;
    }
}

.publication-title {
    margin-top: 1.56rem;
    margin-bottom: 0.63rem;
    display: flex;
    justify-content: space-between;
}

.publication-detail {
    padding-top: 2rem;
    padding-bottom: 1.56rem;
    border-top: 1px solid $lineColor1;
    border-bottom: 1px solid $lineColor1;
    margin-bottom: 1.25rem;
}
</style>
