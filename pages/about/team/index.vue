<template>
    <div class="page-outer">
        <div v-for="(value, key) in teamMembers">
            <h1>{{ key }}</h1>
            <div v-for="person in value">
                <img :src="`${person.profile}/thumbnail`" :alt="person.name" width="180"
                    onerror="this.onerror=null; this.src='../../../static/img/blank-profile-picture.svg';" />
                <a v-if="person.profile" :href="person.profile">{{ person.name }}</a>
                <span v-else>{{ person.name }}</span>
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
        return {
            teamsItems: teamsItems.values,
        };
    },

    data: () => {
        return {};
    },

    computed: {
        principalInvestigator() {
            return this.teamsItems.filter(
                (item) => item.category === "principalInvestigator"
            );
        },
        associateInvestigator() {
            return this.teamsItems.filter(
                (item) => item.category === "associateInvestigator"
            );
        },
        researcher() {
            return this.teamsItems.filter((item) => item.category === "researcher");
        },
        projectManager() {
            return this.teamsItems.filter(
                (item) => item.category === "projectManager"
            );
        },
        specialThanks() {
            return this.teamsItems.filter(
                (item) => item.category === "specialThanks"
            );
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
};
</script>

<style scoped lang="scss"></style>
