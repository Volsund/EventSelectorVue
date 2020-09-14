<template>
    <div id="app">
        <b-container fluid="lg">
            <b-row>
                <b-col lg="3" order-lg="2">
                    <SideBar
                        @newDatePicked="updateDate($event)"
                        @typeUpdate="updateTypes($event)"
                    />
                </b-col>
                <b-col lg="9">
                    <Events :checkedTypes="latestTypes" :newDate="newDate" />
                </b-col>
            </b-row>
        </b-container>
    </div>
</template>

<script>
import Events from "./components/Events.vue";
import SideBar from "./components/SideBar.vue";

export default {
    name: "App",
    components: {
        Events,
        SideBar,
    },
    data() {
        return {
            latestTypes: [],
            newDate: "",
        };
    },

    methods: {
        updateTypes($event) {
            this.latestTypes = $event;
        },
        updateDate($event) {
            this.newDate = $event;
        },
    },
};
</script>

<style lang="scss">
    body {
        color: #131313;
    }
</style>
