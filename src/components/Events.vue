<template>
    <div class="overflow-auto">
        <b-container class="mb-3">
            <b-row class="event-header">
                {{ isDatePicked ? new Date(newDate).toDateString() : 'Visi' }}
            </b-row>
            <b-row id="event-list">
                <b-col
                    class="event-card"
                    v-for="event in paginatedEventsToShow"
                    :key="event.id"
                    md="6"
                    lg="4"
                >
                    <p class="event-date">{{ event.date }}</p>

                    <b-img fluid fluid-grow :src="event.lead_image" alt="" class="event-image" /><br />
                    <p class="event-type">{{ event.type }}</p>
                    <p class="event-title">{{ event.title }}</p>
                </b-col>
            </b-row>

            <b-row>
                <b-pagination
                    v-model="currentPage"
                    :total-rows="currentEventAmount"
                    :per-page="perPage"
                    aria-controls="event-list"
                    pills
                    first-number
                    last-number
                    align="center"
                    class="col-12"
                ></b-pagination>
            </b-row>
        </b-container>
    </div>
</template>

<script>
import json from "@/assets/mock_data2019.json";

export default {
    name: "Events",
    props: ["checkedTypes", "newDate"],
    data: function() {
        return {
            events: json,
            currentPage: 1,
            perPage: 6,
        };
    },
    methods: {
        compareDates(a, b) {
            if (new Date(a.date) < new Date(b.date)) {
                return -1;
            }
            if (new Date(a.date) > new Date(b.date)) {
                return 1;
            }
            return 0;
        },
    },
    computed: {
        isDatePicked() {
            if (typeof this.newDate.getMonth === "function") {
                return true;
            }
            return false;
        },
        eventsToShow() {
            if (
                this.checkedTypes.indexOf("visas") === 0 &&
                this.checkedTypes.length === 1
            ) {
                if (this.isDatePicked) {
                    let allEvents = this.events;
                    let result = allEvents.filter(
                        (event) =>
                            new Date(event.date).getTime() ===
                            this.newDate.getTime()
                    );

                    return result.sort(this.compareDates);
                }
                let eventArr = this.events;
                return eventArr.sort(this.compareDates);
            } else {
                if (this.isDatePicked) {
                    let allEvents = this.events.filter(
                        (event) => this.checkedTypes.indexOf(event.type) > -1
                    );
                    let result = allEvents.filter(
                        (event) =>
                            new Date(event.date).getTime() ===
                            this.newDate.getTime()
                    );

                    return result.sort(this.compareDates);
                }
                let filteredEvents = this.events.filter(
                    (event) => this.checkedTypes.indexOf(event.type) > -1
                );

                return filteredEvents.sort(this.compareDates);
            }
        },
        currentEventAmount() {
            return this.eventsToShow.length;
        },
        paginatedEventsToShow() {
            return this.eventsToShow.slice(
                (this.currentPage - 1) * this.perPage,
                this.currentPage * this.perPage
            );
        },
    },

    mounted: function() {
        //  console.log();
    },
};
</script>

<style lang="scss">
    $color-gray: #888888;

    .event-header {
        text-transform: uppercase;
        font-weight: bold;
        padding: 1rem 0;
        margin-bottom: 1rem;
        border-bottom: 1px solid $color-gray;
    }

    .event {
        &-card,
        &-image {
            margin-bottom: 1rem;
        }
        &-type {
            text-transform: uppercase;
            color: #C9AC38;
            display: flex;

            &:after {
                content: '';
                flex: 1;
                border-bottom: 1px solid #C9AC38;
                margin-bottom: 6px;
                margin-left: 1rem;
            }
        }
        &-title {
            font-weight: bold;
        }
        &-date {
            color: $color-gray;
        }
    }
</style>
