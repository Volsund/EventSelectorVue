<template>
    <aside>
        <b-form class="sidebar">
            <b-form-group>
                <b-form-input placeholder="Meklēt notikumu"></b-form-input>
            </b-form-group>
            <b-form-checkbox-group v-model="checkedTypes" @change="newChange">
                <b-form-group>
                    <b-form-checkbox value="visas">Visas tēmas</b-form-checkbox>
                </b-form-group>
                <b-form-group>
                    <b-form-checkbox value="kultūra"
                        >Izklaide, kultūra</b-form-checkbox
                    >
                </b-form-group>
                <b-form-group>
                    <b-form-checkbox value="sports">Sports</b-form-checkbox>
                </b-form-group>
                <b-form-group>
                    <b-form-checkbox value="ģimenēm"
                        >Ģimenēm ar bērniem</b-form-checkbox
                    >
                </b-form-group>
                <b-form-group>
                    <b-form-checkbox value="izglītība"
                        >Izglītība</b-form-checkbox
                    >
                </b-form-group>
                <b-form-group>
                    <b-form-checkbox value="pilseta-parvalde"
                        >Pilsēta, pārvalde</b-form-checkbox
                    >
                </b-form-group>
                <b-form-group>
                    <b-form-checkbox value="cits">Cita tēma</b-form-checkbox>
                </b-form-group>
            </b-form-checkbox-group>
            <b-row>
                <b-col>
                    <div>
                        <label for="datepicker">Izvēlies datumu:</label>
                        <b-form-datepicker
                            id="datepicker"
                            v-model="calendarValue"
                            :min="minDate"
                            :max="maxDate"
                            class="mb-2"
                            value-as-date
                            @input="datePicked"
                            reset-button
                            @reset="datePicked"
                        ></b-form-datepicker>
                        <!-- <p>Value: '{{ value }}'</p> -->
                    </div>
                </b-col>
            </b-row>
        </b-form>
    </aside>
</template>

<script>
export default {
    name: "SideBar",
    data() {
        return {
            pickedDate: "",
            calendarValue: "",
            minDate: new Date("7/15/2018"),
            maxDate: new Date("7/14/2019"),
            allTypesChecked: true,
            checkedTypes: ["visas"],
        };
    },
    methods: {
        testLog() {
            console.log("potato");
        },
        newChange(events) {
            if (events.length === 0) {
                events.push("visas");
                this.$emit("typeUpdate", events);
            } else {
                if (events.indexOf("visas") > -1) {
                    events.splice(events.indexOf("visas"), 1);
                }
                this.$emit("typeUpdate", events);
            }
        },
        datePicked($event) {
            if ($event) {
                this.pickedDate = $event;
            } else {
                this.pickedDate = "";
            }
            this.$emit("newDatePicked", this.pickedDate);
        },
    },

    mounted() {
        this.$emit("typeUpdate", this.checkedTypes);
    },
};
</script>

<style lang="scss">
.sidebar {
    padding: 2rem;
    background-color: #f1f1f1;
}
</style>
