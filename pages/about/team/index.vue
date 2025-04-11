<template>
    <div class="page-outer">
        <breadcrumb-trail :breadcrumb="breadcrumb" :title="pageTitle" />
        <div class="detail-container">
            <div class="vertical-flex">
                <div class="team-title">
                    <h1>Meet our team</h1>
                </div>
                <div>
                    <iframe src="https://www.youtube.com/embed/1racsLgTLSk" class="video"></iframe>
                </div>
                <div class="team-detail">
                    <div v-for="(value, key) in teamMembers">
                        <h1>{{ key }}</h1>
                        <div class="member-category">
                            <div v-for="member in value" class="curved-box member-card">
                                <object :data="member.imageUrl" type="image/png" class="member-image">
                                    <img :src="defaultImage" :alt="member.name" class="member-image" />
                                </object>
                                <h3 class="member-name">{{ member.name }}</h3>
                                <el-button v-if="member.profile" @click="viewProfile(member.profile)">
                                    PROFILE
                                </el-button>
                            </div>
                        </div>
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
import graphcmsQuery from "@/services/graphcmsQuery";
export default {
    name: "TeamPage",

    async asyncData({ $graphcms }) {
        const teamsItems = await graphcmsQuery.teamsItems($graphcms);
        const teamsEntry = teamsItems.values.map((item) => {
            return {
                ...item,
                imageUrl: `${item.profile}/thumbnail`,
            }
        });
        return {
            teamsItems: teamsEntry,
        };
    },

    data: () => {
        return {
            pageTitle: 'Team',
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
            defaultImage: require('../../../static/img/blank-profile-picture.png'),
        };
    },

    computed: {
        principalInvestigator() {
            return this.teamsItems.filter((item) => {
                return item.category === "principalInvestigator";
            });
        },
        associateInvestigator() {
            return this.teamsItems.filter((item) => {
                return item.category === "associateInvestigator"
            }
            );
        },
        researcher() {
            return this.teamsItems.filter((item) => {
                return item.category === "researcher"
            });
        },
        projectManager() {
            return this.teamsItems.filter((item) => {
                return item.category === "projectManager"
            });
        },
        specialThanks() {
            return this.teamsItems.filter((item) => {
                return item.category === "specialThanks"
            });
        },
        teamMembers() {
            return {
                "Principal Investigator:": this.principalInvestigator,
                "Associate Investigator:": this.associateInvestigator,
                "Researchers:": this.researcher,
                "Project Manager:": this.projectManager,
                "Special Thanks to Contributions from:": this.specialThanks,
            };
        },
    },


    methods: {
        viewProfile(link) {
            window.open(link, "_blank");
        },
    }
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

.team-title {
    margin-top: 1.56rem;
    margin-bottom: 0.63rem;
    display: flex;
    justify-content: space-between;
}

.team-detail {
    padding-top: 2rem;
    padding-bottom: 1.56rem;
    border-top: 1px solid $lineColor1;
    border-bottom: 1px solid $lineColor1;
    margin-bottom: 1.25rem;
}

.member-category {
    display: flex;
    flex-wrap: wrap;
}

.member-card {
    width: 180px;
    display: flex;
    flex-direction: column;
    align-items: center;
    margin: 10px;
    padding: 10px;
}

.member-image {
    width: 180px;
    border-radius: 1.25rem;
}

.member-name {
    margin-top: 1rem;
    height: 3rem;
}

.video {
    width: 100%;
    height: 500px;
}
</style>
