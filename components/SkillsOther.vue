<template>
  <div id="others">
    <client-only placeholder="Loading...">
      <div v-if="selected==='names'" class="d-flex flex-row justify-content-center flex-wrap">
        <div
          v-for="(skill,id) in  $store.getters['skills/sortSkillsByName'].skillsOthers"
          :key="id"
          class="skillOthers"
        >
          <div v-if="skill.note>=noteMin">
            <p v-b-tooltip.hover.top="skill.tooltip">{{capitalize(skill.title)}}</p>

            <apexcharts
              type="radialBar"
              height="300"
              :options="chartOptions"
              :series="[skill.note*10]"
            />
          </div>
        </div>
      </div>
      <div v-else class="d-flex flex-row justify-content-center flex-wrap">
        <div
          v-for="(skill,id) in  $store.getters['skills/sortSkillsByScore'].skillsOthers"
          :key="id"
        >
          <div v-if="skill.note>=noteMin">
            <p>{{capitalize(skill.title)}}</p>
            <apexcharts
              type="radialBar"
              height="300"
              :options="chartOptions"
              :series="[skill.note*10]"
            />
          </div>
        </div>
      </div>
    </client-only>
  </div>
</template>

  <script>
export default {
  components: {
    Apexcharts: () => import("vue-apexcharts")
  },
  props: {
    selected: { type: String, required: true },
    noteMin: { type: Number, required: true }
  },
  data: function() {
    return {
      chartOptions: {
        chart: {
          height: 280,
          type: "radialBar"
        },

        series: [67],
        colors: ["#20E647"],
        plotOptions: {
          radialBar: {
            hollow: {
              margin: 0,
              size: "70%",
              background: "#293450"
            },
            track: {
              dropShadow: {
                enabled: true,
                top: 2,
                left: 0,
                blur: 4,
                opacity: 0.15
              }
            },
            dataLabels: {
              name: {
                offsetY: -10,
                color: "#fff",
                fontSize: "1.5rem",
                show: false
              },
              value: {
                color: "#fff",
                fontSize: "2rem",
                show: true
              }
            }
          }
        },
        fill: {
          type: "gradient",
          gradient: {
            shade: "dark",
            type: "vertical",
            gradientToColors: ["#87D4F9"],
            stops: [0, 100]
          }
        },
        stroke: {
          lineCap: "round"
        }
      }
    };
  },
  methods: {
    capitalize(title) {
      let newStr = "";

      title = title.split(" ");

      title.forEach(word => {
        newStr += word[0].toUpperCase() + word.slice(1).toLowerCase() + " ";
      });
      return newStr;
    }
  }
};
</script>

  <style>
#others p {
  color: white;
  font-size: 1.2rem;
  text-align: center;
  z-index: 99;
  text-shadow: 0 0 1px black;
  max-width: 15rem;
  margin: auto;
}
.skillOthers {
  max-width: 30rem;
  display: flex;
  height: 20rem;
  flex-direction: column;
  justify-content: flex-end;
}
#others {
  background-image: linear-gradient(
      rgba(0, 0, 0, 0.5),
      rgba(255, 255, 255, 0.2)
    ),
    url("../assets/img/otherBackground.jpg");
  background-repeat: no-repeat;

  background-size: cover;
  background-position-y: center;
  padding: 1rem;
  margin: 2rem 0;
}
</style>
