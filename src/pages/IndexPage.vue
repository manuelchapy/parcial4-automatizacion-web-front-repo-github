<template>
  <q-page class="flex flex-center">
    <div class="fit row">
      <div class="col-6">
        <apexchart
          width="500"
          type="donut"
          :options="cantUsosNombres"
          :series="cantUsosValores"
        ></apexchart>
      </div>
      <div class="col-6">
        <apexchart
          width="500"
          type="bar"
          :options="cantTiempoNombres"
          :series="cantTiempoValores"
        ></apexchart>
      </div>
      <div class="col-12">
        <apexchart
          width="1300"
          type="line"
          :options="RelacionNombres"
          :series="RelacionesValores"
        ></apexchart>
      </div>
    </div>
  </q-page>
</template>

<script>
import axios from "axios";
import { ref } from "vue";

export default {
  data() {
    let cantUsosNombres = ref(["0"]);
    let cantUsosValores = ref([0]);
    let cantTiempoNombres = ref({});
    let cantTiempoValores = ref([0]);
    let RelacionNombres = ref({});
    let RelacionesValores = ref([0]);

    let ip = "https://parcial4-webapp-telemetria-e1ab55d3e238.herokuapp.com/";

    axios.get(ip + "conteoCantidadDeUsosMarca").then((response) => {
      console.log(response.data);
      cantUsosNombres.value = { labels: response.data.nombres_reactivo };
      cantUsosValores.value = response.data.usos_por_reactivo;

      //console.log(cantUsosNombres.value);
    });

    axios.get(ip + "deconstrucciondataGraficoUsoTiempo").then((response) => {
      console.log(response.data);
      RelacionNombres.value = {
        yaxis: {
          title: {
            text: "Veces de uso",
          },
          min: 65,
          max: 180,
        },
      };
      RelacionesValores.value = response.data.series;

      //console.log(cantUsosNombres.value);
    });

    axios.get(ip + "conteoCantidadDeTiempoMarca").then((response) => {
      console.log(response.data);
      cantTiempoNombres.value = {
        xaxis: {
          categories: response.data.nombres_reactivo,
        },
      };
      cantTiempoValores.value = [
        {
          name: "tiempo de uso",
          data: response.data.tiempo_por_reactivo,
        },
      ];

      //console.log(cantUsosNombres.value);
    });

    console.log("!!!!", cantTiempoNombres.value, cantTiempoValores.value);

    return {
      cantUsosValores,
      cantUsosNombres,
      cantTiempoNombres,
      cantTiempoValores,
      RelacionNombres,
      RelacionesValores,
      /*cantTiempoNombres: {
        xaxis: {
          categories: [
            "1991",
            "1992",
            "1993",
            "1994",
            "1995",
            "1996",
            "1997",
            "1998",
          ],
        },
      },
      cantTiempoValores: [
        {
          data: [30, 40, 45, 50, 49, 60, 70, 91],
        },
      ],*/
    };
  },
};
</script>
