<template>
  <div class="control-panel section">
    <header class="animate__animated animate__tada animate__fast">
      <h2>Control Panel</h2>
    </header>

    <main>
      <div
        class="control-panel__select animate__animated animate__fadeInDown animate__slow animate__delay-0.9s"
      >
        <v-container fluid>
          <v-row align="center">
            <v-col class="d-flex">
              <v-select
                :items="pressureOptions"
                :return-object="true"
                item-text="value"
                v-model="selectedPressureOption"
                background-color="#D9E5F0"
                label="Select: Tires Pressure"
                solo
              >
                <v-icon slot="append">mdi-arrow-down-circle-outline</v-icon>
              </v-select>
            </v-col>
          </v-row>
        </v-container>
      </div>

      <div
        class="control-panel__wheels animate__animated animate__fadeInDown animate__slow animate__delay-0.9s"
      >
        <span
          v-for="poption in pressureOptions"
          :key="poption.value"
          :class="
            poption.position +
              ' animate__animated animate__zoomInLeft animate__slow animate__delay-1s ' +
              poption.animation
          "
        >
          {{ poption.value }} BAR
        </span>

        <img
          src="../assets/wheel-chassis.png"
          class="chassis animate__animated animate__zoomIn animate__slow animate__delay-2s"
        />

        <WheelsSVG :selectedPressureOption="selectedPressureOption" />
      </div>

      <div
        class="control-panel__alerts animate__animated animate__fadeInDown animate__slow animate__delay-0.9s"
      >
        <v-card class="mx-auto" elevation="12">
          <v-card-text>
            <div class="text">
              <v-icon>mdi-map-marker-alert-outline</v-icon>Check your front
              right tire!
            </div>
          </v-card-text>
        </v-card>
      </div>

      <div
        class="control-panel__switches  animate__animated animate__fadeInDown animate__slow animate__delay-0.9s"
      >
        <div class="control-panel__switches--row">
          <div class="switch">
            <v-switch
              v-model="switchLocked"
              :label="switchLocked ? 'Locked' : 'Unlocked'"
              hide-details
            ></v-switch>
            <v-icon class="left">mdi-lock-open</v-icon>
            <v-icon class="right">mdi-lock</v-icon>
          </div>

          <div class="switch">
            <v-switch
              v-model="switchAlarm"
              :label="switchAlarm ? 'Alarm On' : 'Alarm Off'"
              hide-details
            ></v-switch>
            <v-icon class="left">mdi-alarm-off</v-icon>
            <v-icon class="right">mdi-alarm</v-icon>
          </div>
        </div>
        <div class="control-panel__switches--row">
          <div class="switch">
            <v-switch
              v-model="switchLights"
              :label="switchLights ? 'Lights On' : 'Lights Off'"
              hide-details
            ></v-switch>
            <v-icon class="left">mdi-lightbulb-off</v-icon>
            <v-icon class="right">mdi-lightbulb-on</v-icon>
          </div>

          <div class="switch">
            <v-switch
              v-model="switchHeating"
              :label="switchHeating ? 'Heating On' : 'Heating Off'"
              hide-details
            ></v-switch>
            <v-icon class="left">mdi-thermometer-low</v-icon>
            <v-icon class="right">mdi-thermometer-high</v-icon>
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
// external
import axios from "axios";

// components
import WheelsSVG from "../components/WheelsSVG";

// import dummy-data json file
import DUMMY_DATA from "../assets/json/dummy-data.json";

export default {
  name: "ControlPanel",

  components: { WheelsSVG },

  dummyJsonDataWayOne: DUMMY_DATA,

  data: () => ({
    userData: [],
    pressureOptions: [
      { position: "top-left", value: 1.0, animation: "animate__zoomInLeft" },
      { position: "top-right", value: 2.0, animation: "animate__zoomInRight" },
      { position: "bottom-left", value: 3.0, animation: "animate__zoomInLeft" },
      {
        position: "bottom-right",
        value: 4.0,
        animation: "animate__zoomInRight",
      },
    ],
    selectedPressureOption: null,
    switchLocked: true,
    switchAlarm: false,
    switchLights: false,
    switchHeating: true,
    dummyJsonDataWayTwo: DUMMY_DATA,
  }),

  created() {
    this.getUserData();
    this.getDummyDataFromJsonFile();
    this.setDefaultPressureOption();
  },

  methods: {
    getUserData() {
      axios({
        url: process.env.VUE_APP_API_URL + "/users",
        method: "GET",
      }).then((resp) => {
        console.log(
          "--- Get Users using Lodash :: ",
          this.$_.get(resp, "data")
        );
      });
    },
    getDummyDataFromJsonFile() {
      console.log(
        "--- dummyJsonDataWayOne :: ",
        this.$options.dummyJsonDataWayOne
      );
      console.log("--- dummyJsonDataWayTwo :: ", this.dummyJsonDataWayTwo);
    },
    setDefaultPressureOption() {
      this.selectedPressureOption = this.pressureOptions[0];
    },
  },
};
</script>

<style lang="scss">
@import "../scss/main.scss";

.control-panel {
  height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;

  header {
    color: $primary-color;
  }

  &__select {
    min-width: 400px;

    .v-select__slot {
      .v-label {
        color: $primary-color;
        font-weight: 500;
      }
    }
  }

  &__wheels {
    position: relative;
    color: $gray;

    .chassis {
      transform: scale(1.5);
      position: absolute;
      top: 30%;
      left: 45%;
    }

    .top-left {
      position: absolute;
      top: -10px;
      left: 1%;
    }

    .top-right {
      position: absolute;
      right: 2%;
      top: -6%;
    }

    .bottom-left {
      position: absolute;
      top: 44%;
      left: 1%;
    }

    .bottom-right {
      position: absolute;
      right: 2%;
      top: 44%;
    }
  }

  &__alerts {
    min-width: 400px;
    padding-top: 30px;

    .v-card.v-sheet.theme--light {
      background-color: $element;
    }

    .text {
      display: flex;
      align-items: center;
      justify-content: center;
      color: $primary-color;
      font-weight: 500;
      font-size: $font-size-medium;

      .v-icon {
        margin-right: 10px;
        color: $orange;
      }
    }
  }

  &__switches {
    @extend %switch-theme;
    padding: 30px 0;

    &--row {
      display: flex;
      justify-content: space-evenly;
      padding: 10px 0;
    }

    .switch {
      position: relative;
      width: 145px;

      .v-icon {
        position: absolute;
        top: 30px;
        color: $primary-color;
        font-size: 20px;
      }

      .v-icon.left {
        right: 32px;
      }
      .v-icon.right {
        right: 0px;
      }
    }
  }
}
</style>
