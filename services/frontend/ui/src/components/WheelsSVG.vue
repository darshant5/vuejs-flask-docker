<template>
  <div>
    <div>
      <svg width="400" height="80" viewBox="0 0 680 100">
        <filter
          id="fillTopRect"
          primitiveUnits="objectBoundingBox"
          x="0%"
          y="0%"
          width="100%"
          height="100%"
        >
          <feFlood
            x="0%"
            y="0%"
            width="100%"
            height="100%"
            flood-color="#ED8138"
          />
          <feOffset :dy="fillTopValue">
            <animate
              ref="animateTopRect"
              attributeName="dy"
              from="1"
              :to="fillTopValue"
              dur="2s"
            />
          </feOffset>
          <feComposite operator="in" in2="SourceGraphic" />
          <feComposite operator="over" in2="SourceGraphic" />
        </filter>

        <polyline points="5,5 100,5 140,60 190,60" :style="getPolylineStyle" />

        <rect
          x="200"
          y="20"
          rx="15"
          ry="15"
          width="50"
          height="80"
          :filter="filterTopLeftRect"
          :style="getRectStyle"
        />

        <rect
          x="420"
          y="20"
          rx="15"
          ry="15"
          width="50"
          height="80"
          :filter="filterTopRightRect"
          :style="getRectStyle"
        />

        <polyline
          points="480,60 530,60 570,5 665,5"
          :style="getPolylineStyle"
        />
        Sorry, your browser does not support inline SVG.
      </svg>
    </div>
    <div>
      <svg width="400" height="80" viewBox="0 0 680 100">
        <filter
          id="fillBottomRect"
          primitiveUnits="objectBoundingBox"
          x="0%"
          y="0%"
          width="100%"
          height="100%"
        >
          <feFlood
            x="0%"
            y="0%"
            width="100%"
            height="100%"
            flood-color="#ED8138"
          />
          <feOffset :dy="fillBottomValue">
            <animate
              ref="animateBottomRect"
              attributeName="dy"
              from="1"
              :to="fillBottomValue"
              dur="2s"
            />
          </feOffset>
          <feComposite operator="in" in2="SourceGraphic" />
          <feComposite operator="over" in2="SourceGraphic" />
        </filter>

        <polyline points="5,5 100,5 140,60 190,60" :style="getPolylineStyle" />

        <rect
          x="200"
          y="20"
          rx="15"
          ry="15"
          width="50"
          height="80"
          :filter="filterBottomLeftRect"
          :style="getRectStyle"
        />

        <rect
          x="420"
          y="20"
          rx="15"
          ry="15"
          width="50"
          height="80"
          :filter="filterBottomRightRect"
          :style="getRectStyle"
        />

        <polyline
          points="480,60 530,60 570,5 665,5"
          :style="getPolylineStyle"
        />
        Sorry, your browser does not support inline SVG.
      </svg>
    </div>
  </div>
</template>

<script>
export default {
  name: "WheelsSVG",

  props: ["areaPosition", "selectedPressureOption"],

  data: () => ({
    filterTopLeftRect: undefined,
    filterTopRightRect: undefined,
    filterBottomLeftRect: undefined,
    filterBottomRightRect: undefined,
    fillTopValue: undefined,
    fillBottomValue: undefined,
  }),

  mounted: function() {
    this.highlightSVGRect(this.selectedPressureOption);
  },

  methods: {
    highlightSVGRect(data) {
      this.resetFillRectValues();

      const position = this.$_.get(data, "position");
      const value = Math.abs(this.$_.get(data, "value") / 5 - 1);

      switch (position) {
        case "top-left":
          this.filterTopLeftRect = "url(#fillTopRect)";
          this.fillTopValue = value;
          break;
        case "top-right":
          this.filterTopRightRect = "url(#fillTopRect)";
          this.fillTopValue = value;
          break;
        case "bottom-left":
          this.filterBottomLeftRect = "url(#fillBottomRect)";
          this.fillBottomValue = value;
          break;
        case "bottom-right":
          this.filterBottomRightRect = "url(#fillBottomRect)";
          this.fillBottomValue = value;
          break;
        default:
          break;
      }

      this.animateRect(position);
    },
    resetFillRectValues() {
      this.filterTopLeftRect = undefined;
      this.filterTopRightRect = undefined;
      this.filterBottomLeftRect = undefined;
      this.filterBottomRightRect = undefined;
      this.fillTopValue = undefined;
      this.fillBottomValue = undefined;
    },
    animateRect(position) {
      if (position === "top-left" || position === "top-right") {
        this.$refs.animateTopRect.beginElement();
      } else {
        this.$refs.animateBottomRect.beginElement();
      }
    },
  },

  computed: {
    getPolylineStyle() {
      return "fill:none;stroke:#c8cccf;stroke-width:4";
    },
    getRectStyle() {
      return "fill:#FFFFFF;stroke:#DFE3E6;stroke-width:1;opacity:1";
    },
  },

  watch: {
    selectedPressureOption: {
      handler(data) {
        this.highlightSVGRect(data);
      },
      deep: true,
    },
  },
};
</script>
