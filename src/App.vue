<template><v-app>
  <v-main>
    <v-container class="mt-16">

      <v-row class="mb-2">
        <v-col cols="12">
          <v-btn href="https://juandsm27.github.io/portfolio/#tools" variant="text" prepend-icon="mdi-arrow-left" class="back-btn">Portfolio</v-btn>
        </v-col>
      </v-row>
      <v-row class="mb-6">
        <v-col cols="12">
          <h1 class="calculator-title">Dividend Calculator</h1>
        </v-col>
      </v-row>

      <v-row>

        <!-- INPUTS -->
        <v-col cols="12" md="6">

          <v-row>
            <v-col class="col-style">
              <v-text-field :prefix="currency" density="compact" type="number" v-model.number="inversionIni" label="Inversión Inicial" variant="outlined" @input="onChange($event)"></v-text-field>
            </v-col>
          </v-row>
          <v-row>
            <v-col class="col-style">
              <v-text-field :prefix="currency" density="compact" type="number" v-model.number="precio" label="Precio por Acción" variant="outlined" @input="onChange($event)"></v-text-field>
            </v-col>
          </v-row>
          <v-row>
            <v-col class="col-style">
              <v-text-field :prefix="percent" density="compact" type="number" v-model.number="crecimientoAcc" label="Crecimiento de la Acción (Anual)" variant="outlined" @input="onChange($event)"></v-text-field>
            </v-col>
          </v-row>
          <v-row>
            <v-col class="col-style">
              <v-text-field :prefix="currency" density="compact" type="number" v-model.number="dividendo" label="Dividendo por Acción (Anual)" variant="outlined" @input="onChange($event)"></v-text-field>
            </v-col>
          </v-row>
          <v-row>
            <v-col class="col-style">
              <v-text-field :prefix="percent" density="compact" type="number" v-model.number="crecimientoDiv" label="Crecimiento del Diviendo (Anual)" variant="outlined" @input="onChange($event)"></v-text-field>
            </v-col>
          </v-row>
          <v-row>
            <v-col class="col-style">
              <v-text-field :prefix="currency" density="compact" type="number" v-model.number="contribuciones" label="Contribución (Anual)" variant="outlined" @input="onChange($event)"></v-text-field>
            </v-col>
          </v-row>
          <v-row>
            <v-col class="col-style">
              <v-text-field :prefix="percent" density="compact" type="number" v-model.number="impuestos" label="Impuestos Sobre Dividendos" variant="outlined" @input="onChange($event)"></v-text-field>
            </v-col>
          </v-row>
          <v-row>
            <v-col class="col-style" id="years">
              <v-text-field density="compact" type="number" v-model.number="years" label="Años" variant="outlined" v-on:input="() => { if(years > 100 || years < 0) { years = 100 }}" @input="onChange($event)"></v-text-field>
            </v-col>
          </v-row>
          <v-row>
            <v-col class="col-style">
              <v-select
                v-model="calcularcos"
                :menu-props="{ 'max-height': 500 }"
                :items="itemsCalc"
                item-title="state"
                item-value="valorComp"
                label="Reinversión"
                return-object
                variant="outlined"
                density="compact" 
              ></v-select>
            </v-col>
          </v-row>

        </v-col>

        <!-- RESULTADOS -->
        <v-col cols="12" md="6">

          <v-row class="px-md-10">
            <v-col cols="12" class="calculator-investment">
              <div class="monthly-payment-mount">{{ currency }}{{ formattedNumber(this.balanceFinal.toFixed(2)) }}</div>
              <v-divider class="monthly-payment-line"></v-divider>
              <h1 class="monthly-payment-title">Balance Final</h1>
            </v-col>
          </v-row>
          <v-row class="px-md-10">
            <v-col cols="12" class="calculator-investment">
              <div class="monthly-payment-mount">{{ currency }}{{ formattedNumber(this.inversionTotal.toFixed(2)) }}</div>
              <v-divider class="monthly-payment-line"></v-divider>
              <h1 class="monthly-payment-title">Inversión Total</h1>
            </v-col>
          </v-row>
          <v-row class="px-md-10">
            <v-col cols="12" class="calculator-investment">
              <div class="monthly-payment-mount">{{ currency }}{{ formattedNumber(this.divDistribucion.toFixed(2)) }}</div>
              <v-divider class="monthly-payment-line"></v-divider>
              <h1 class="monthly-payment-title">Dividendos Recibidos</h1>
            </v-col>
          </v-row>
          <v-row class="px-md-10">
            <v-col cols="12" class="calculator-investment">
              <div class="monthly-payment-mount">{{ currency }}{{ formattedNumber(this.apreciacionInv.toFixed(2)) }}</div>
              <v-divider class="monthly-payment-line"></v-divider>
              <h1 class="monthly-payment-title">Apreciación</h1>
            </v-col>
          </v-row>
          <!-- <v-row class="px-md-10">
            <v-col cols="12" class="calculator-investment">
              <div class="monthly-payment-mount">{{ currency }}{{ formattedNumber(this.contribucionTotal.toFixed(2)) }}</div>
              <v-divider class="monthly-payment-line"></v-divider>
              <h1 class="monthly-payment-title">Contribuciones</h1>
            </v-col>
          </v-row> -->
          <v-row class="px-md-10">
            <v-col cols="12" class="calculator-investment">
              <div v-if="this.impuestosShow >= 0" class="monthly-payment-mount">{{ currency }}{{ formattedNumber(this.impuestosShow.toFixed(2)) }}</div>
              <div v-if="this.impuestosShow < 0" class="monthly-payment-mount">-{{ currency }}{{ formattedNumber(this.impuestosShow.toFixed(2)*(-1)) }}</div>
              <v-divider class="monthly-payment-line"></v-divider>
              <h1 class="monthly-payment-title">Impuestos</h1>
            </v-col>
          </v-row>

        </v-col>

        <!-- BARRAS -->
          <v-col cols="12">
            <h1 class="titlepie"> BALANCES </h1>
            <v-divider class="monthly-payment-line"></v-divider>
            <div id="barchartdiv" style="width: 100%; height: 500px;"></div>
          </v-col>
          <v-col cols="12">
            <h1 class="titlepie">Apreciación y Distribución de dividendos</h1>
            <v-divider class="monthly-payment-line"></v-divider>
            <div id="barchartdiv2" style="width: 100%; height: 500px;"></div>
          </v-col>
          <v-col cols="12">
            <h1 class="titlepie"> RENDIMIENTOS </h1>
            <v-divider class="monthly-payment-line"></v-divider>
            <div id="barchartdiv3" style="width: 100%; height: 500px;"></div>
          </v-col>
          <v-col cols="12">
            <h1 class="titlepie"> DIVIDENDOS (Con y sin Reinversión) </h1>
            <v-divider class="monthly-payment-line"></v-divider>
            <div id="barchartdiv4" style="width: 100%; height: 500px;"></div>
          </v-col>

        <!-- TABLAS -->
        <v-row id="tables">
          <v-col cols="12">
            <v-card>
              <v-tabs class="tabtext" v-model="tab" bg-color="#b54914">
                <v-tab value="anual">Tabla Anual</v-tab>
                <v-tab value="impcont">Impuestos y Contribuciones</v-tab>
              </v-tabs>
              <v-card-text>
                <v-window v-model="tab"> 
                  <v-window-item value="anual">
                    <v-table fixed-header height="580px">
                      <thead>
                        <tr>
                          <th class="text-left">Año</th>
                          <th class="text-left">Balance Inicial</th>
                          <th class="text-left">Precio Acción</th>
                          <th class="text-left">Número Acciones</th>
                          <th class="text-left">Dividendo Acción</th>
                          <th class="text-left">Distribución Dividendo</th>
                          <th class="text-left">Dividendo Total</th>
                          <th class="text-left">Rendimiento Dividendo</th>
                          <th class="text-left">Apreciación Acción</th>
                          <th class="text-left">Apreciación Inversión</th>
                          <th class="text-left">Apreciación Total</th>
                          <th class="text-left">Balance Final</th>
                          <th class="text-left">Rendimiento Sobre&nbsp;Costo</th>
                        </tr>
                      </thead>
                      <tbody>
                        <tr v-for="item in tablaAnual" :key="item.name">
                          <td>{{ item.año }}</td>
                          <td>{{ formattedNumber(item.balanceIni) }}</td>
                          <td>{{ formattedNumber(item.precioAcc) }}</td>
                          <td>{{ formattedNumber(item.numAcciones) }}</td>
                          <td>{{ formattedNumber(item.divAccion) }}</td>
                          <td>{{ formattedNumber(item.divDistribucion) }}</td>
                          <td>{{ formattedNumber(item.divTotal) }}</td>
                          <td>{{ formattedNumber(item.divRendimiento) }}</td>
                          <td>{{ formattedNumber(item.apreciacionAcc) }}</td>
                          <td>{{ formattedNumber(item.apreciacionInv) }}</td>
                          <td>{{ formattedNumber(item.apreciacionTotal) }}</td>
                          <td>{{ formattedNumber(item.balanceFinal) }}</td>
                          <td>{{ formattedNumber(item.rendimientoCosto) }}</td>
                        </tr>
                      </tbody>
                    </v-table>
                  </v-window-item>
                  <v-window-item value="impcont">
                    <v-table fixed-header height="580px">
                      <thead>
                        <tr>
                          <th class="text-left">Años</th>
                          <th class="text-left">Contribución Total</th>
                          <th class="text-left">Impuestos</th>
                          <th class="text-left">Impuestos Total</th>
                        </tr>
                      </thead>
                      <tbody>
                        <tr v-for="item in tablaImpuestos" :key="item.name">
                          <td>{{ item.año }}</td>
                          <td>{{ formattedNumber(item.contribucion_total) }}</td>
                          <td>{{ formattedNumber(item.impuestos) }}</td>
                          <td>{{ formattedNumber(item.impuestos_total) }}</td>
                        </tr> 
                      </tbody>
                    </v-table>
                  </v-window-item>
                </v-window>
              </v-card-text>
            </v-card>
          </v-col>
        </v-row>     

      </v-row>
    </v-container>
  </v-main>
</v-app>
</template>

<script>
import * as am4core from "@amcharts/amcharts4/core";
import * as am4charts from "@amcharts/amcharts4/charts";
import am4themes_animated from "@amcharts/amcharts4/themes/animated";
am4core.useTheme(am4themes_animated);

export default {
  name: 'App',  
  data() {
    return {
      percent: "%",
      currency: "$",
      validationError: false,
      tab: null,
      inversionIni: 0,
      dividendo: 0,
      crecimientoDiv: 0,
      precio: 0,
      crecimientoAcc: 0,
      contribuciones: 0,
      impuestos: 0,
      years: 0,
      balanceIni: 0,
      numAcciones: 0,
      divAccion: 0,
      divDistribucion: 0,
      divTotal: 0,
      divRendimiento: 0,
      apreciacionAcc: 0,
      apreciacionInv: 0,
      apreciacionTotal: 0,
      balanceFinal: 0,
      rendimientoCosto: 0,
      precioAcc: 0,
      inversionTotal: 0,
      tablaAnual: [],
      tablaImpuestos: [],
      chartBarData: [],
      chartBarData2: [],
      chartBarData3: [],
      chartBarData4: [],
      contribucionTotal: 0,
      impuestosShow: 0,
      calcularcos: { state: 'Con Reinversión', valorComp: 1 },
        itemsCalc: [
          { state: 'Con Reinversión', valorComp: 1 },
          { state: 'Sin Reinversión', valorComp: 2 },
        ],
    };
  },
  watch: {
    calcularcos: function () {
      this.onChange();
    },
  },

  methods: {
    onChange() {
      if (
        this.inversionIni !== "" &&
        this.divAccion !== "" &&
        this.crecimientoDiv !== "" &&
        this.precio !== "" &&
        this.precio > 0  &&
        this.contribuciones !== "" &&
        this.impuestos !== "" &&
        this.años !== ""
      ) {
        this.validationError = false
        this.calculate();
      } else {
        this.validationError = true
      }
    },
    calculate() {
      const currentDate = new Date();
      let actYear = currentDate.getFullYear();
      this.tablaAnual = [];
      this.tablaImpuestos = [];
      const yearsQuantity = this.years;
      let balanceIni = 0;
      let precioAcc = 0;
      let numAcciones = 0;
      let divAccion = 0;
      let divDistribucion = 0;
      let divTotal = 0;
      let divRendimiento = 0;
      let apreciacionInv = 0;
      let balanceFinal = 0;
      let rendimientoCosto = 0;
      let apreciacionTotal = 0;
      let apreciacionAcc = 0;
      let impuestos = this.impuestos/100;
      let crecimientoAcc = this.crecimientoAcc/100;
      let crecimientoDiv = this.crecimientoDiv/100;
      let contribuciones = this.contribuciones;
      this.chartBarData = [];
      this.chartBarData2 = [];
      this.chartBarData3 = [];
      this.chartBarData4 = [];
      if ( this.calcularcos.valorComp === 1) {

      // "Balance Inicial"
      balanceIni = this.inversionIni;
      // "Precio por Acción"
      precioAcc = this.precio;
      // "Número de Acciones"
      numAcciones = balanceIni/precioAcc;
      // "Dividendo por Acción"
      divAccion = this.dividendo;
      // "Distribución de Dividendo"
      divDistribucion = divAccion*numAcciones-(divAccion*numAcciones)*impuestos;
      // "Dividendo Total"
      divTotal = divDistribucion;
      // "Rendimiento por Dividendo"
      divRendimiento = (divDistribucion/balanceIni)*100;
      // "Apreciación por Acción"
      apreciacionAcc = (this.precio*Math.pow(1+crecimientoAcc, 1))-this.precio;
      // "Apreciación por Inversión"
      apreciacionInv = apreciacionAcc*numAcciones;
      // "Apreciación Total"
      apreciacionTotal = apreciacionInv;
      // "Balance Final"
      balanceFinal = ((precioAcc+apreciacionAcc)*numAcciones)+divDistribucion;
      // "Rendimiento Sobre Costo"
      rendimientoCosto = (divTotal/this.inversionIni)*100;
      //Impuestos y Contribuciones
      let contribucionTotal = 0;
      let impuestosTabla = (divAccion*numAcciones)*impuestos*-1;
      let impuestosTotal = impuestosTabla;
      this.chartBarData = [];
      this.chartBarData2 = [];
      this.chartBarData3 = [];
      this.chartBarData4 = [];
      this.tablaImpuestos.push({ 
      año: 1, 
      contribucion_total: contribucionTotal.toFixed(2),
      impuestos: impuestosTabla.toFixed(2),
      impuestos_total: impuestosTotal.toFixed(2),
      });

      this.tablaAnual.push({ 
      año: 1, 
      balanceIni: balanceIni.toFixed(2),
      precioAcc: precioAcc.toFixed(2),
      numAcciones: numAcciones.toFixed(2),
      divAccion: divAccion.toFixed(2),
      divDistribucion: divDistribucion.toFixed(2),
      divTotal: divTotal.toFixed(2),
      divRendimiento: divRendimiento.toFixed(2),
      apreciacionAcc: apreciacionAcc.toFixed(2),
      apreciacionInv: apreciacionInv.toFixed(2),
      apreciacionTotal: apreciacionTotal.toFixed(2),
      balanceFinal: balanceFinal.toFixed(2),
      rendimientoCosto: rendimientoCosto.toFixed(2),
      });
      //Resultados
      this.divDistribucion = divDistribucion;
      this.apreciacionInv = apreciacionInv;

      //BARRACALC
      this.chartBarData.push({
        "date": actYear.toString(),
        "inversioninicial": this.inversionIni.toFixed(),
        "contribuciones": contribucionTotal.toFixed(),
        "dividendos": divTotal.toFixed(2),
        "apreciacion": apreciacionTotal.toFixed(2),
        "impuestos": impuestosTotal.toFixed(2),
        "balance": balanceFinal.toLocaleString('en-US', {
          style: 'currency',
          currency: 'USD', // Cambia a tu moneda deseada (ejemplo: 'EUR', 'JPY', 'GBP', etc.)
          minimumFractionDigits: 2,
          maximumFractionDigits: 2,
        }),
      });
      this.chartBarData2.push({
        "date": actYear.toString(),
        "apreciacionB": apreciacionInv.toFixed(2),
        "dividendosB": divDistribucion.toFixed(2),
        "total": (apreciacionInv+divDistribucion).toLocaleString('en-US', {
              style: 'currency',
              currency: 'USD', // Cambia a tu moneda deseada (ejemplo: 'EUR', 'JPY', 'GBP', etc.)
              minimumFractionDigits: 2,
              maximumFractionDigits: 2,
            }),
      });
      this.chartBarData3.push({
        "date": actYear.toString(),
        "rendimientodiv": divRendimiento.toFixed(2),
        "rendimientocosto": rendimientoCosto.toFixed(2),
      });


      //Ultima Tabla Con Reinversion
      
      let balanceIniSR = 0;
      let precioAccSR = 0;
      let numAccionesSR = 0;
      let divAccionSR = 0;
      let divDistribucionSR = 0;
      let divTotalSR = 0;
      let apreciacionInvSR = 0;
      let balanceFinalSR = 0;
      let apreciacionTotalSR = 0;
      let apreciacionAccSR = 0;
      let impuestosSR = this.impuestos/100;
      let crecimientoAccSR = this.crecimientoAcc/100;
      let crecimientoDivSR = this.crecimientoDiv/100;
      let contribucionesSR = this.contribuciones;
      // "Balance Inicial"
      balanceIniSR = this.inversionIni;
      // "Precio por Acción"
      precioAccSR = this.precio;
      // "Número de Acciones"
      numAccionesSR = balanceIniSR/precioAccSR;
      // "Dividendo por Acción"
      divAccionSR = this.dividendo;
      // "Distribución de Dividendo"
      divDistribucionSR = divAccionSR*numAccionesSR-(divAccionSR*numAccionesSR)*impuestosSR;
      // "Dividendo Total"
      divTotalSR = divDistribucionSR;
      // "Rendimiento por Dividendo"
      // "Apreciación por Acción"
      apreciacionAccSR = (this.precio*Math.pow(1+crecimientoAccSR, 1))-this.precio;
      // "Apreciación por Inversión"
      apreciacionInvSR = apreciacionAccSR*numAccionesSR;
      // "Apreciación Total"
      apreciacionTotalSR = apreciacionInvSR;
      // "Balance Final"
      balanceFinalSR = ((precioAccSR+apreciacionAccSR)*numAccionesSR);
      // "Rendimiento Sobre Costo"
      //Impuestos y Contribuciones
      //Resultados
      this.divDistribucion = divDistribucionSR;
      this.apreciacionInv = apreciacionInvSR;

      this.chartBarData4.push({
        "date": actYear.toString(),
        "balanceFinalCR": balanceFinal.toFixed(2),
        "balanceFinalSR": balanceFinalSR.toFixed(2),
        "dividendosCR": divTotal.toFixed(2),
        "dividendosSR": divTotalSR.toFixed(2),
      });

      //Ultima Tabla Con Reinversion


      actYear++;
      this.chartBar.data = this.chartBarData;
      this.chartBar2.data = this.chartBarData2;
      this.chartBar3.data = this.chartBarData3;
      this.chartBar4.data = this.chartBarData4;

      for (let index = 2; index <= yearsQuantity; index++) {
        
      // "Balance Inicial"
      balanceIni = balanceFinal+contribuciones;
      // "Precio por Acción"
      precioAcc = precioAcc*Math.pow((1+crecimientoAcc), 1);
      // "Número de Acciones"
      numAcciones = balanceIni/precioAcc;
      // "Dividendo por Acción"
      divAccion = divAccion*Math.pow((1+crecimientoDiv), 1);
      // "Distribución de Dividendo"
      divDistribucion = divAccion*numAcciones-(divAccion*numAcciones)*impuestos;
      // "Dividendo Total"
      divTotal = divDistribucion+divTotal;
      // "Rendimiento por Dividendo"
      divRendimiento = (divDistribucion/balanceIni)*100;
      // "Apreciación por Acción"
      apreciacionAcc = (precioAcc*Math.pow(1+crecimientoAcc, 1))-precioAcc;
      // "Apreciación por Inversión"
      apreciacionInv = apreciacionAcc*numAcciones;
      // "Apreciación Total"
      apreciacionTotal = apreciacionInv+apreciacionTotal;
      // "Balance Final"
      balanceFinal = ((precioAcc+apreciacionAcc)*numAcciones)+divDistribucion;
      // "Rendimiento Sobre Costo"
      contribucionTotal += contribuciones;
      rendimientoCosto = (divDistribucion/(this.inversionIni+contribucionTotal))*100;


      impuestosTabla = (divAccion*numAcciones)*impuestos*-1;
      impuestosTotal += impuestosTabla;

      
      this.chartBarData.push({
        "date": actYear.toString(),
        "inversioninicial": this.inversionIni.toFixed(),
        "contribuciones": contribucionTotal.toFixed(),
        "dividendos": divTotal.toFixed(2),
        "apreciacion": apreciacionTotal.toFixed(2),
        "impuestos": impuestosTotal.toFixed(2),
        "balance": balanceFinal.toLocaleString('en-US', {
          style: 'currency',
          currency: 'USD', // Cambia a tu moneda deseada (ejemplo: 'EUR', 'JPY', 'GBP', etc.)
          minimumFractionDigits: 2,
          maximumFractionDigits: 2,
        }),
      });

      this.chartBarData2.push({
        "date": actYear.toString(),
        "apreciacionB": apreciacionInv.toFixed(2),
        "dividendosB": divDistribucion.toFixed(2),
        "total": (apreciacionInv+divDistribucion).toLocaleString('en-US', {
              style: 'currency',
              currency: 'USD', // Cambia a tu moneda deseada (ejemplo: 'EUR', 'JPY', 'GBP', etc.)
              minimumFractionDigits: 2,
              maximumFractionDigits: 2,
            }),
      });

      this.chartBarData3.push({
        "date": actYear.toString(),
        "rendimientodiv": divRendimiento.toFixed(2),
        "rendimientocosto": rendimientoCosto.toFixed(2),
      });

    //Ultima Tabla Con Reinversion

    // "Balance Inicial"
    balanceIniSR = balanceFinalSR+contribucionesSR;
    // "Precio por Acción"
    precioAccSR = precioAccSR*Math.pow((1+crecimientoAccSR), 1);
    // "Número de Acciones"
    numAccionesSR = balanceIniSR/precioAccSR;
    // "Dividendo por Acción"
    divAccionSR = divAccionSR*Math.pow((1+crecimientoDivSR), 1);
    // "Distribución de Dividendo"
    divDistribucionSR = divAccionSR*numAccionesSR-(divAccionSR*numAccionesSR)*impuestosSR;
    // "Dividendo Total"
    divTotalSR = divDistribucionSR+divTotalSR;
    // "Rendimiento por Dividendo"
    // "Apreciación por Acción"
    apreciacionAccSR = (precioAccSR*Math.pow(1+crecimientoAccSR, 1))-precioAccSR;
    // "Apreciación por Inversión"
    apreciacionInvSR = apreciacionAccSR*numAccionesSR;
    // "Apreciación Total"
    apreciacionTotalSR = apreciacionInvSR+apreciacionTotalSR;
    // "Balance Final"
    balanceFinalSR = ((precioAccSR+apreciacionAccSR)*numAccionesSR);
    // "Rendimiento Sobre Costo"

          this.chartBarData4.push({
            "date": actYear.toString(),
            "balanceFinalSR": balanceFinalSR.toFixed(2),
            "balanceFinalCR": balanceFinal.toFixed(2),
            "dividendosSR": divTotalSR.toFixed(2),
            "dividendosCR": divTotal.toFixed(2),
          });

      //Ultima Tabla Con Reinversion

      this.tablaImpuestos.push({ 
      año: index, 
      contribucion_total: contribucionTotal.toFixed(2),
      impuestos: impuestosTabla.toFixed(2),
      impuestos_total: impuestosTotal.toFixed(2),
      });
          this.tablaAnual.push({ 
          año: index, 
          balanceIni: balanceIni.toFixed(2),
          precioAcc: precioAcc.toFixed(2),
          numAcciones: numAcciones.toFixed(2),
          divAccion: divAccion.toFixed(2),
          divDistribucion: divDistribucion.toFixed(2),
          divTotal: divTotal.toFixed(2),
          divRendimiento: divRendimiento.toFixed(2),
          apreciacionAcc: apreciacionAcc.toFixed(2),
          apreciacionInv: apreciacionInv.toFixed(2),
          apreciacionTotal: apreciacionTotal.toFixed(2),
          balanceFinal: balanceFinal.toFixed(2),
          rendimientoCosto: rendimientoCosto.toFixed(2),
        });
      //Resultados
      this.inversionTotal = this.inversionIni+contribucionTotal;
      this.balanceFinal = balanceFinal;
      this.divDistribucion += divDistribucion;
      this.apreciacionInv += apreciacionInv;
      this.contribucionTotal = contribucionTotal;
      this.impuestosShow = impuestosTotal;
      actYear++;
      }
    }

    
    else if ( this.calcularcos.valorComp === 2) {

      // "Balance Inicial"
      balanceIni = this.inversionIni;
      // "Precio por Acción"
      precioAcc = this.precio;
      // "Número de Acciones"
      numAcciones = balanceIni/precioAcc;
      // "Dividendo por Acción"
      divAccion = this.dividendo;
      // "Distribución de Dividendo"
      divDistribucion = divAccion*numAcciones-(divAccion*numAcciones)*impuestos;
      // "Dividendo Total"
      divTotal = divDistribucion;
      // "Rendimiento por Dividendo"
      divRendimiento = (divDistribucion/balanceIni)*100;
      // "Apreciación por Acción"
      apreciacionAcc = (this.precio*Math.pow(1+crecimientoAcc, 1))-this.precio;
      // "Apreciación por Inversión"
      apreciacionInv = apreciacionAcc*numAcciones;
      // "Apreciación Total"
      apreciacionTotal = apreciacionInv;
      // "Balance Final"
      balanceFinal = ((precioAcc+apreciacionAcc)*numAcciones);
      // "Rendimiento Sobre Costo"
      rendimientoCosto = (divTotal/this.inversionIni)*100;
      //Impuestos y Contribuciones
      let contribucionTotal = 0;
      let impuestosTabla = (divAccion*numAcciones)*impuestos*-1;
      let impuestosTotal = impuestosTabla;
      this.chartBarData = [];
      this.chartBarData2 = [];
      this.chartBarData3 = [];
      this.tablaImpuestos.push({ 
      año: 1, 
      contribucion_total: contribucionTotal.toFixed(2),
      impuestos: impuestosTabla.toFixed(2),
      impuestos_total: impuestosTotal.toFixed(2),
      });

      this.tablaAnual.push({ 
      año: 1, 
      balanceIni: balanceIni.toFixed(2),
      precioAcc: precioAcc.toFixed(2),
      numAcciones: numAcciones.toFixed(2),
      divAccion: divAccion.toFixed(2),
      divDistribucion: divDistribucion.toFixed(2),
      divTotal: divTotal.toFixed(2),
      divRendimiento: divRendimiento.toFixed(2),
      apreciacionAcc: apreciacionAcc.toFixed(2),
      apreciacionInv: apreciacionInv.toFixed(2),
      apreciacionTotal: apreciacionTotal.toFixed(2),
      balanceFinal: balanceFinal.toFixed(2),
      rendimientoCosto: rendimientoCosto.toFixed(2),
      });
      //Resultados
      this.divDistribucion = divDistribucion;
      this.apreciacionInv = apreciacionInv;

      //BARRACALC
      this.chartBarData.push({
        "date": actYear.toString(),
        "inversioninicial": this.inversionIni.toFixed(),
        "contribuciones": contribucionTotal.toFixed(),
        "dividendos": divTotal.toFixed(2),
        "apreciacion": apreciacionTotal.toFixed(2),
        "impuestos": impuestos.toFixed(2),
        "balance": balanceFinal.toLocaleString('en-US', {
          style: 'currency',
          currency: 'USD', // Cambia a tu moneda deseada (ejemplo: 'EUR', 'JPY', 'GBP', etc.)
          minimumFractionDigits: 2,
          maximumFractionDigits: 2,
        }),
      });
      this.chartBarData2.push({
        "date": actYear.toString(),
        "apreciacionB": apreciacionInv.toFixed(2),
        "dividendosB": divDistribucion.toFixed(2),
        "total": (apreciacionInv+divDistribucion).toLocaleString('en-US', {
              style: 'currency',
              currency: 'USD', // Cambia a tu moneda deseada (ejemplo: 'EUR', 'JPY', 'GBP', etc.)
              minimumFractionDigits: 2,
              maximumFractionDigits: 2,
            }),
      });
      this.chartBarData3.push({
        "date": actYear.toString(),
        "rendimientodiv": divRendimiento.toFixed(2),
        "rendimientocosto": rendimientoCosto.toFixed(2),
      });

      //Ultima Tabla Con Reinversion
      
      let balanceIniSR = 0;
      let precioAccSR = 0;
      let numAccionesSR = 0;
      let divAccionSR = 0;
      let divDistribucionSR = 0;
      let divTotalSR = 0;
      let apreciacionInvSR = 0;
      let balanceFinalSR = 0;
      let apreciacionTotalSR = 0;
      let apreciacionAccSR = 0;
      let impuestosSR = this.impuestos/100;
      let crecimientoAccSR = this.crecimientoAcc/100;
      let crecimientoDivSR = this.crecimientoDiv/100;
      let contribucionesSR = this.contribuciones;
      // "Balance Inicial"
      balanceIniSR = this.inversionIni;
      // "Precio por Acción"
      precioAccSR = this.precio;
      // "Número de Acciones"
      numAccionesSR = balanceIniSR/precioAccSR;
      // "Dividendo por Acción"
      divAccionSR = this.dividendo;
      // "Distribución de Dividendo"
      divDistribucionSR = divAccionSR*numAccionesSR-(divAccionSR*numAccionesSR)*impuestosSR;
      // "Dividendo Total"
      divTotalSR = divDistribucionSR;
      // "Rendimiento por Dividendo"
      // "Apreciación por Acción"
      apreciacionAccSR = (this.precio*Math.pow(1+crecimientoAccSR, 1))-this.precio;
      // "Apreciación por Inversión"
      apreciacionInvSR = apreciacionAccSR*numAccionesSR;
      // "Apreciación Total"
      apreciacionTotalSR = apreciacionInvSR;
      // "Balance Final"
      balanceFinalSR = ((precioAccSR+apreciacionAccSR)*numAccionesSR)+divDistribucionSR;
      // "Rendimiento Sobre Costo"
      //Impuestos y Contribuciones
      //Resultados
      this.divDistribucion = divDistribucionSR;
      this.apreciacionInv = apreciacionInvSR;

      this.chartBarData4.push({
        "date": actYear.toString(),
        "balanceFinalCR": balanceFinalSR.toFixed(2),
        "balanceFinalSR": balanceFinal.toFixed(2),
        "dividendosCR": divTotalSR.toFixed(2),
        "dividendosSR": divTotal.toFixed(2),
      });
      //Ultima Tabla Con Reinversion

      actYear++;
      this.chartBar.data = this.chartBarData;
      this.chartBar2.data = this.chartBarData2;
      this.chartBar3.data = this.chartBarData3;
      this.chartBar4.data = this.chartBarData4;

      for (let index = 2; index <= yearsQuantity; index++) {
        
      // "Balance Inicial"
      balanceIni = balanceFinal+contribuciones;
      // "Precio por Acción"
      precioAcc = precioAcc*Math.pow((1+crecimientoAcc), 1);
      // "Número de Acciones"
      numAcciones = balanceIni/precioAcc;
      // "Dividendo por Acción"
      divAccion = divAccion*Math.pow((1+crecimientoDiv), 1);
      // "Distribución de Dividendo"
      divDistribucion = divAccion*numAcciones-(divAccion*numAcciones)*impuestos;
      // "Dividendo Total"
      divTotal = divDistribucion+divTotal;
      // "Rendimiento por Dividendo"
      divRendimiento = (divDistribucion/balanceIni)*100;
      // "Apreciación por Acción"
      apreciacionAcc = (precioAcc*Math.pow(1+crecimientoAcc, 1))-precioAcc;
      // "Apreciación por Inversión"
      apreciacionInv = apreciacionAcc*numAcciones;
      // "Apreciación Total"
      apreciacionTotal = apreciacionInv+apreciacionTotal;
      // "Balance Final"
      balanceFinal = ((precioAcc+apreciacionAcc)*numAcciones);
      // "Rendimiento Sobre Costo"
      contribucionTotal += contribuciones;
      rendimientoCosto = (divDistribucion/(this.inversionIni+contribucionTotal))*100;


      impuestosTabla = (divAccion*numAcciones)*impuestos*-1;
      impuestosTotal += impuestosTabla;


      this.chartBarData.push({
        "date": actYear.toString(),
        "inversioninicial": this.inversionIni.toFixed(),
        "contribuciones": contribucionTotal.toFixed(),
        "dividendos": divTotal.toFixed(2),
        "apreciacion": apreciacionTotal.toFixed(2),
        "impuestos": impuestos.toFixed(2),
        "balance": balanceFinal.toLocaleString('en-US', {
          style: 'currency',
          currency: 'USD', // Cambia a tu moneda deseada (ejemplo: 'EUR', 'JPY', 'GBP', etc.)
          minimumFractionDigits: 2,
          maximumFractionDigits: 2,
        }),
      });

      this.chartBarData2.push({
        "date": actYear.toString(),
        "apreciacionB": apreciacionInv.toFixed(2),
        "dividendosB": divDistribucion.toFixed(2),
        "total": (apreciacionInv+divDistribucion).toLocaleString('en-US', {
              style: 'currency',
              currency: 'USD', // Cambia a tu moneda deseada (ejemplo: 'EUR', 'JPY', 'GBP', etc.)
              minimumFractionDigits: 2,
              maximumFractionDigits: 2,
            }),
      });

      this.chartBarData3.push({
        "date": actYear.toString(),
        "rendimientodiv": divRendimiento.toFixed(2),
        "rendimientocosto": rendimientoCosto.toFixed(2),
      });


    //Ultima Tabla Con Reinversion

    // "Balance Inicial"
    balanceIniSR = balanceFinalSR+contribucionesSR;
    // "Precio por Acción"
    precioAccSR = precioAccSR*Math.pow((1+crecimientoAccSR), 1);
    // "Número de Acciones"
    numAccionesSR = balanceIniSR/precioAccSR;
    // "Dividendo por Acción"
    divAccionSR = divAccionSR*Math.pow((1+crecimientoDivSR), 1);
    // "Distribución de Dividendo"
    divDistribucionSR = divAccionSR*numAccionesSR-(divAccionSR*numAccionesSR)*impuestosSR;
    // "Dividendo Total"
    divTotalSR = divDistribucionSR+divTotalSR;
    // "Rendimiento por Dividendo"
    // "Apreciación por Acción"
    apreciacionAccSR = (precioAccSR*Math.pow(1+crecimientoAccSR, 1))-precioAccSR;
    // "Apreciación por Inversión"
    apreciacionInvSR = apreciacionAccSR*numAccionesSR;
    // "Apreciación Total"
    apreciacionTotalSR = apreciacionInvSR+apreciacionTotalSR;
    // "Balance Final"
    balanceFinalSR = ((precioAccSR+apreciacionAccSR)*numAccionesSR)+divDistribucionSR;
    // "Rendimiento Sobre Costo"

          this.chartBarData4.push({
            "date": actYear.toString(),
            "balanceFinalSR": balanceFinal.toFixed(2),
            "balanceFinalCR": balanceFinalSR.toFixed(2),
            "dividendosSR": divTotal.toFixed(2),
            "dividendosCR": divTotalSR.toFixed(2),
          });

      //Ultima Tabla Con Reinversion

      this.tablaImpuestos.push({ 
      año: index, 
      contribucion_total: contribucionTotal.toFixed(2),
      impuestos: impuestosTabla.toFixed(2),
      impuestos_total: impuestosTotal.toFixed(2),
      });
          this.tablaAnual.push({ 
          año: index, 
          balanceIni: balanceIni.toFixed(2),
          precioAcc: precioAcc.toFixed(2),
          numAcciones: numAcciones.toFixed(2),
          divAccion: divAccion.toFixed(2),
          divDistribucion: divDistribucion.toFixed(2),
          divTotal: divTotal.toFixed(2),
          divRendimiento: divRendimiento.toFixed(2),
          apreciacionAcc: apreciacionAcc.toFixed(2),
          apreciacionInv: apreciacionInv.toFixed(2),
          apreciacionTotal: apreciacionTotal.toFixed(2),
          balanceFinal: balanceFinal.toFixed(2),
          rendimientoCosto: rendimientoCosto.toFixed(2),
        });
      //Resultados
      this.inversionTotal = this.inversionIni+contribucionTotal;
      this.balanceFinal = balanceFinal;
      this.divDistribucion += divDistribucion;
      this.apreciacionInv += apreciacionInv;
      this.contribucionTotal = contribucionTotal;
      this.impuestosShow = impuestosTotal;
      actYear++;
      }
    }
    },
    formattedNumber(number) {
      return number.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",");
    },
  },
  mounted() {
    //BAR CHART
    this.chartBar = am4core.create("barchartdiv", am4charts.XYChart);
      if (this.chartBar.logo) {
        this.chartBar.logo.dispose()
      }
      this.chartBar.background.fill = am4core.color("#0f0f18");
      this.chartBar.background.fillOpacity = 1;
      this.chartBar.data = [{
        "date": "",
        "inversioninicial": 0,
        "contribuciones": 0,
        "dividendos": 0,
        "apreciacion": 0,
        "impuestos": 0,
        "balance": 0,
      }];
      // AXIS
      var categoryAxis = this.chartBar.xAxes.push(new am4charts.CategoryAxis());
      categoryAxis.dataFields.category = "date";
      categoryAxis.renderer.grid.template.disabled = true;
      categoryAxis.renderer.labels.template.fill = am4core.color("#6b6b80");

      var axisTooltip = categoryAxis.tooltip;
      axisTooltip.background.fill = am4core.color("#000");
      axisTooltip.background.strokeWidth = 0;
      axisTooltip.background.cornerRadius = 5;
      axisTooltip.background.pointerLength = 0;
      axisTooltip.dy = 5;
      axisTooltip.animationDuration = 0;

      var  valueAxis = this.chartBar.yAxes.push(new am4charts.ValueAxis());
      valueAxis.numberFormatter.numberFormat = "'$'#,###.##a|'-$'#,###.##as";
      valueAxis.tooltip.disabled = true;
      valueAxis.renderer.labels.template.fill = am4core.color("#6b6b80");
      valueAxis.renderer.grid.template.stroke = am4core.color("#ffffff");
      valueAxis.renderer.grid.template.strokeOpacity = 0.05;
      valueAxis.numberFormatter.numberFormat = "'$'#,###.##a|'-$'#,###.##as";
      valueAxis.tooltip.disabled = true;
      valueAxis.numberFormatter.bigNumberPrefixes = [
        // { "number": 1e+3, "suffix": "K" },
        // { "number": 1e+6, "suffix": "M" },
        { "number": 1e+9, "suffix": "B" },
        { "number": 1e+12, "suffix": "T" }
      ];
      valueAxis.numberFormatter.bigNumberThreshold = "1e+3";
      valueAxis.numberFormatter.smallNumberThreshold = "0.00";
      valueAxis.numberFormatter.smallNumberPrefixes = [
        { "number": 1e-12, "suffix": "" },
        { "number": 1e-9, "suffix": "" },
        { "number": 1e-6, "suffix": "" },
        { "number": 1e-3, "suffix": "" },
        { "number": 1e-1, "suffix": "" },
        { "number": 0, "suffix": "" },
        { "number": 1, "suffix": "" },
      ];

      // COLORES
      this.chartBar.colors.list = [
        am4core.color("#7c6af7"),
        am4core.color("#4ed8c0"),
        am4core.color("#f06fba"),
        am4core.color("#38bdf8"),
        am4core.color("#fb923c"),
      ];


      // INVERSION INICIAL
      var series = this.chartBar.series.push(new am4charts.ColumnSeries());
      series.dataFields.valueY = "inversioninicial";
      series.dataFields.categoryX = "date";
      series.name = "Inversión Inicial";
      series.tooltip.animationDuration = 0;
      series.tooltipText = "[#fff]{name}: [bold, #fff]{valueY}[/]";
      series.tooltip.background.filters.clear();
      series.stacked = true;

      // CONTRIBUCIONES
      var series2 = this.chartBar.series.push(new am4charts.ColumnSeries());
      series2.dataFields.valueY = "contribuciones";
      series2.dataFields.categoryX = "date";
      series2.name = "Contribuciones";
      series2.tooltip.animationDuration = 0;
      series2.tooltipText = "[#fff]{name}: [bold, #fff]{valueY}[/]";
      series2.tooltip.background.filters.clear();
      series2.stacked = true;

      // DIVIDENDOS
      var series3 = this.chartBar.series.push(new am4charts.ColumnSeries());
      series3.dataFields.valueY = "dividendos";
      series3.dataFields.categoryX = "date";
      series3.name = "Dividendos";
      series3.tooltip.animationDuration = 0;
      series3.tooltipText = "{name}: [bold]{valueY}[/]";
      series3.tooltip.background.filters.clear();
      series3.stacked = true;

      // APRECIACION
      var series4 = this.chartBar.series.push(new am4charts.ColumnSeries());
      series4.dataFields.valueY = "apreciacion";
      series4.dataFields.categoryX = "date";
      series4.name = "Apreciación";
      series4.tooltip.animationDuration = 0;
      series4.tooltipText = "{name}: [bold]{valueY}[/]";
      series4.tooltip.background.filters.clear();
      series4.stacked = true;
      series4.columns.template.column.cornerRadiusTopLeft = 5;
      series4.columns.template.column.cornerRadiusTopRight = 5;

      // IMPUESTOS
      var series5 = this.chartBar.series.push(new am4charts.ColumnSeries());
      series5.dataFields.valueY = "impuestos";
      series5.dataFields.categoryX = "date";
      series5.name = "Impuestos";
      series5.tooltip.animationDuration = 0;
      series5.tooltipText = "{name}: [bold]{valueY}[/]";
      series5.tooltip.background.filters.clear();
      series5.stacked = true;
      series5.columns.template.column.cornerRadiusBottomLeft = 5;
      series5.columns.template.column.cornerRadiusBottomRight = 5;
      series5.hidden = true;


      // CURSOR
      this.chartBar.cursor = new am4charts.XYCursor();
      this.chartBar.cursor.behavior = "none";
      this.chartBar.cursor.lineY.disabled = true;
      //this.chartBar.cursor.lineX.disabled = true;
      this.chartBar.cursor.fullWidthLineX = true;
      this.chartBar.cursor.xAxis = categoryAxis;
      this.chartBar.cursor.lineX.strokeOpacity = 0;
      this.chartBar.cursor.lineX.fill = "#000";
      this.chartBar.cursor.lineX.fillOpacity = 0.1;

      // LEYENDA
      this.chartBar.legend = new am4charts.Legend();
      this.chartBar.legend.labels.template.fill = am4core.color("#eeeef6");

      // BOTON ZOOM-OUT
      this.chartBar.zoomOutButton.disabled = true;


      // TOTAL
      var categoryAxis2 = this.chartBar.xAxes.push(new am4charts.CategoryAxis());
      categoryAxis2.renderer.grid.template.disabled = true;
      categoryAxis2.renderer.opposite = true;
      categoryAxis2.dataFields.category = "date";
      categoryAxis2.renderer.grid.disabled = true;
      categoryAxis2.renderer.labels.template.disabled = true;
      categoryAxis2.adapter.add("getTooltipText", () => {
        return "{balance}";
      });

      axisTooltip = categoryAxis2.tooltip;
      axisTooltip.background.fill = am4core.color("#000");
      axisTooltip.background.strokeWidth = 0;
      axisTooltip.background.cornerRadius = 5;
      axisTooltip.background.pointerLength = 0;
      axisTooltip.dy = -5;
      axisTooltip.animationDuration = 0;

      this.chartBar.topAxesContainer.paddingTop = 30;


      // RESPONSIVE
      this.chartBar.responsive.enabled = true;
      this.chartBar.responsive.useDefault = false

      this.chartBar.responsive.rules.push({
        relevant: function(target) {
          if (target.pixelWidth <= 500) {
            return true;
          }
          
          return false;
        },
        state: function(target, stateId) {
          
          if (target instanceof am4charts.Chart) {
            state = target.states.create(stateId);
            state.properties.paddingTop = 0;
            state.properties.paddingRight = 0;
            state.properties.paddingBottom = 0;
            state.properties.paddingLeft = 0;
            return state;
          }
          
          if (target instanceof am4charts.Legend) {
            state = target.states.create(stateId);
            state.properties.paddingTop = 0;
            state.properties.paddingRight = 0;
            state.properties.paddingBottom = 0;
            state.properties.paddingLeft = 0;
            state.properties.marginLeft = 0;
            return state;
          }
          
          if (target instanceof am4charts.AxisRendererY) {
            state = target.states.create(stateId);
            state.properties.maxLabelPosition = 1;
            return state;
          }
          
          if ((target instanceof am4charts.AxisLabel) && (target.parent instanceof am4charts.AxisRendererY)) { 
            var state = target.states.create(stateId);
            state.properties.dy = 0;
            state.properties.paddingTop = 0;
            state.properties.paddingRight = 0;
            state.properties.paddingBottom = 0;
            state.properties.paddingLeft = 0;
            
            return state;
          }
          
          return null;
        }
      });

    //BAR CHART 2
    this.chartBar2 = am4core.create("barchartdiv2", am4charts.XYChart);
      if (this.chartBar2.logo) {
        this.chartBar2.logo.dispose()
      }
      this.chartBar2.background.fill = am4core.color("#0f0f18");
      this.chartBar2.background.fillOpacity = 1;
      this.chartBar2.data = [{
        "date": "",
        "apreciacionB": 0,
        "dividendosB": 0,
        "total": 0,
      }];
      // AXIS
      var categoryAxisB = this.chartBar2.xAxes.push(new am4charts.CategoryAxis());
      categoryAxisB.dataFields.category = "date";
      categoryAxisB.renderer.grid.template.disabled = true;
      categoryAxisB.renderer.minGridDistance = 50;
      categoryAxisB.renderer.labels.template.fill = am4core.color("#6b6b80");

      var axisTooltipB = categoryAxisB.tooltip;
      axisTooltipB.background.fill = am4core.color("#000");
      axisTooltipB.background.strokeWidth = 0;
      axisTooltipB.background.cornerRadius = 5;
      axisTooltipB.background.pointerLength = 0;
      axisTooltipB.dy = 5;
      axisTooltipB.animationDuration = 0;

      var valueAxisB = this.chartBar2.yAxes.push(new am4charts.ValueAxis());
      valueAxisB.numberFormatter.numberFormat = "'$'#,###.00a|'-$'#,###.00as";
      valueAxisB.tooltip.disabled = true;
      valueAxisB.renderer.labels.template.fill = am4core.color("#6b6b80");
      valueAxisB.renderer.grid.template.stroke = am4core.color("#ffffff");
      valueAxisB.renderer.grid.template.strokeOpacity = 0.05;
      valueAxisB.numberFormatter.numberFormat = "'$'#,###.##a|'-$'#,###.##as";
      valueAxisB.tooltip.disabled = true;
      valueAxisB.numberFormatter.bigNumberPrefixes = [
        // { "number": 1e+3, "suffix": "K" },
        // { "number": 1e+6, "suffix": "M" },
        { "number": 1e+9, "suffix": "B" },
        { "number": 1e+12, "suffix": "T" }
      ];
      valueAxisB.numberFormatter.bigNumberThreshold = "1e+3";
      valueAxisB.numberFormatter.smallNumberThreshold = "0.00";
      valueAxisB.numberFormatter.smallNumberPrefixes = [
        { "number": 1e-12, "suffix": "" },
        { "number": 1e-9, "suffix": "" },
        { "number": 1e-6, "suffix": "" },
        { "number": 1e-3, "suffix": "" },
        { "number": 1e-1, "suffix": "" },
        { "number": 0, "suffix": "" },
        { "number": 1, "suffix": "" },
      ];

      // COLORES
      this.chartBar2.colors.list = [
        am4core.color("#7c6af7"),
        am4core.color("#4ed8c0"),
      ];


      // APRECIACION
      var seriesB = this.chartBar2.series.push(new am4charts.ColumnSeries());
      seriesB.dataFields.valueY = "apreciacionB";
      seriesB.dataFields.categoryX = "date";
      seriesB.name = "Apreciación";
      seriesB.tooltip.animationDuration = 0;
      seriesB.tooltipText = "[#fff]{name}: [bold, #fff]{valueY}[/]";
      seriesB.tooltip.background.filters.clear();
      seriesB.stacked = true;

      // DIVIDENDOS
      var series2B = this.chartBar2.series.push(new am4charts.ColumnSeries());
      series2B.dataFields.valueY = "dividendosB";
      series2B.dataFields.categoryX = "date";
      series2B.name = "Dividendos";
      series2B.tooltip.animationDuration = 0;
      series2B.tooltipText = "[#fff]{name}: [bold, #fff]{valueY}[/]";
      series2B.tooltip.background.filters.clear();
      series2B.stacked = true;
      series2B.columns.template.column.cornerRadiusTopLeft = 5;
      series2B.columns.template.column.cornerRadiusTopRight = 5;


      // CURSOR
      this.chartBar2.cursor = new am4charts.XYCursor();
      this.chartBar2.cursor.behavior = "none";
      this.chartBar2.cursor.lineY.disabled = true;
      this.chartBar2.cursor.fullWidthLineX = true;
      this.chartBar2.cursor.xAxis = categoryAxisB;
      this.chartBar2.cursor.lineX.strokeOpacity = 0;
      this.chartBar2.cursor.lineX.fill = "#000";
      this.chartBar2.cursor.lineX.fillOpacity = 0.1;

      // LEYENDA
      this.chartBar2.legend = new am4charts.Legend();
      this.chartBar2.legend.labels.template.fill = am4core.color("#eeeef6");

      // BOTON ZOOM-OUT
      this.chartBar2.zoomOutButton.align = "left";
      this.chartBar2.zoomOutButton.valign = "top";
      this.chartBar2.zoomOutButton.background.defaultState.properties.fill = "#000";
      this.chartBar2.zoomOutButton.background.defaultState.properties.fillOpacity = .2;
      this.chartBar2.zoomOutButton.background.states.getKey("hover").properties.fill = "#000";
      this.chartBar2.zoomOutButton.background.states.getKey("down").properties.fill = "#000";
      this.chartBar2.zoomOutButton.icon.strokeWidth = 2;


      // TOTAL
      categoryAxis2 = this.chartBar2.xAxes.push(new am4charts.CategoryAxis());
      categoryAxis2.renderer.grid.template.disabled = true;
      categoryAxis2.renderer.opposite = true;
      categoryAxis2.dataFields.category = "date";
      categoryAxis2.renderer.grid.disabled = true;
      categoryAxis2.renderer.labels.template.disabled = true;
      categoryAxis2.adapter.add("getTooltipText", () => {
        return "{total}";
      });

      axisTooltipB = categoryAxis2.tooltip;
      axisTooltipB.background.fill = am4core.color("#000");
      axisTooltipB.background.strokeWidth = 0;
      axisTooltipB.background.cornerRadius = 5;
      axisTooltipB.background.pointerLength = 0;
      axisTooltipB.dy = -5;
      axisTooltipB.animationDuration = 0;

      this.chartBar2.topAxesContainer.paddingTop = 30;


      // RESPONSIVE
      this.chartBar2.responsive.enabled = true;
      this.chartBar2.responsive.useDefault = false

      this.chartBar2.responsive.rules.push({
        relevant: function(target) {
          if (target.pixelWidth <= 500) {
            return true;
          }
          
          return false;
        },
        state: function(target, stateId) {
          
          if (target instanceof am4charts.Chart) {
            var state = target.states.create(stateId);
            state.properties.paddingTop = 0;
            state.properties.paddingRight = 0;
            state.properties.paddingBottom = 0;
            state.properties.paddingLeft = 0;
            return state;
          }

          if (target instanceof am4charts.Legend) {
            state = target.states.create(stateId);
            state.properties.paddingTop = 0;
            state.properties.paddingRight = 0;
            state.properties.paddingBottom = 0;
            state.properties.paddingLeft = 0;
            state.properties.marginLeft = 0;
            return state;
          }
          
          if (target instanceof am4charts.AxisRendererY) {
            state = target.states.create(stateId);
            //state.properties.inside = true;
            state.properties.maxLabelPosition = 1;
            return state;
          }
          
          if ((target instanceof am4charts.AxisLabel) && (target.parent instanceof am4charts.AxisRendererY)) { 
            state = target.states.create(stateId);
            state.properties.dy = 0;
            state.properties.paddingTop = 0;
            state.properties.paddingRight = 0;
            state.properties.paddingBottom = 0;
            state.properties.paddingLeft = 0;
            
            return state;
          }
          
          return null;
        }
      });

    //BAR CHART 3
    this.chartBar3 = am4core.create("barchartdiv3", am4charts.XYChart);
      if (this.chartBar3.logo) {
        this.chartBar3.logo.dispose()
      }
      this.chartBar3.background.fill = am4core.color("#0f0f18");
      this.chartBar3.background.fillOpacity = 1;
      this.chartBar3.data = [{
        "date": "",
        "rendimientodiv": 0,
        "rendimientocosto": 0,
      }];
      // AXIS
      var categoryAxis3 = this.chartBar3.xAxes.push(new am4charts.CategoryAxis());
      categoryAxis3.dataFields.category = "date";
      categoryAxis3.renderer.grid.template.disabled = true;
      categoryAxis3.renderer.minGridDistance = 50;
      categoryAxis3.renderer.labels.template.fill = am4core.color("#6b6b80");

      var axisTooltip3 = categoryAxis3.tooltip;
      axisTooltip3.background.fill = am4core.color("#000");
      axisTooltip3.background.strokeWidth = 0;
      axisTooltip3.background.cornerRadius = 5;
      axisTooltip3.background.pointerLength = 0;
      axisTooltip3.dy = 5;
      axisTooltip3.animationDuration = 0;

      var valueAxis3 = this.chartBar3.yAxes.push(new am4charts.ValueAxis());
      valueAxis3.numberFormatter.numberFormat = "#,###.##a'%'|'-'#,###.##as'%'";
      valueAxis3.tooltip.disabled = true;
      valueAxis3.renderer.labels.template.fill = am4core.color("#6b6b80");
      valueAxis3.renderer.grid.template.stroke = am4core.color("#ffffff");
      valueAxis3.renderer.grid.template.strokeOpacity = 0.05;
      valueAxis3.numberFormatter.numberFormat = "'$'#,###.##a|'-$'#,###.##as";
      valueAxis3.tooltip.disabled = true;
      valueAxis3.numberFormatter.bigNumberPrefixes = [
        // { "number": 1e+3, "suffix": "K" },
        // { "number": 1e+6, "suffix": "M" },
        { "number": 1e+9, "suffix": "B" },
        { "number": 1e+12, "suffix": "T" }
      ];
      valueAxis3.numberFormatter.bigNumberThreshold = "1e+3";
      valueAxis3.numberFormatter.smallNumberThreshold = "0.00";
      valueAxis3.numberFormatter.smallNumberPrefixes = [
        { "number": 1e-12, "suffix": "" },
        { "number": 1e-9, "suffix": "" },
        { "number": 1e-6, "suffix": "" },
        { "number": 1e-3, "suffix": "" },
        { "number": 1e-1, "suffix": "" },
        { "number": 0, "suffix": "" },
        { "number": 1, "suffix": "" },
      ];

      // COLORES
      this.chartBar3.colors.list = [
        am4core.color("#4ed8c0"),
        am4core.color("#f06fba"),
      ];

      // DIVIDEND YIELD
      var series3C = this.chartBar3.series.push(new am4charts.LineSeries());
      series3C.dataFields.valueY = "rendimientodiv";
      series3C.dataFields.categoryX = "date";
      series3C.name = "Rendimiento\npor Dividendo";
      series3C.tooltip.animationDuration = 0;
      series3C.strokeWidth = 3;
      series3C.tooltipText = "Rendimiento\npor Dividendo\n[bold, #fff, font-size:20px]{valueY.formatNumber('#,###.##')}%[/]";
      series3C.tooltip.background.filters.clear();

      var bullet = series3C.bullets.push(new am4charts.CircleBullet());
      bullet.circle.fill = am4core.color("#fff");
      bullet.circle.strokeWidth = 3;

      // YIELD ON COST
      var series4C = this.chartBar3.series.push(new am4charts.LineSeries());
      series4C.dataFields.valueY = "rendimientocosto";
      series4C.dataFields.categoryX = "date";
      series4C.name = "Rendimiento\nsobre Costo";
      series4C.tooltip.animationDuration = 0;
      series4C.strokeWidth = 3;
      series4C.tooltipText = "Rendimiento\nsobre Costo:\n[bold, #000, font-size:20px]{valueY.formatNumber('#,###.##')}%[/]";
      series4C.tooltip.background.filters.clear();

      bullet = series4C.bullets.push(new am4charts.CircleBullet());
      bullet.circle.fill = am4core.color("#fff");
      bullet.circle.strokeWidth = 3;

      // CURSOR
      this.chartBar3.cursor = new am4charts.XYCursor();
      this.chartBar3.cursor.behavior = "none";
      this.chartBar3.cursor.lineY.disabled = true;
      this.chartBar3.cursor.fullWidthLineX = true;
      this.chartBar3.cursor.xAxis = categoryAxis3;
      this.chartBar3.cursor.lineX.strokeOpacity = 0;
      this.chartBar3.cursor.lineX.fill = "#000";
      this.chartBar3.cursor.lineX.fillOpacity = 0.1;



      // LEYENDA
      this.chartBar3.legend = new am4charts.Legend();
      this.chartBar3.legend.labels.template.fill = am4core.color("#eeeef6");

      // BOTON ZOOM-OUT
      this.chartBar3.zoomOutButton.align = "left";
      this.chartBar3.zoomOutButton.valign = "top";
      this.chartBar3.zoomOutButton.background.defaultState.properties.fill = "#000";
      this.chartBar3.zoomOutButton.background.defaultState.properties.fillOpacity = .2;
      this.chartBar3.zoomOutButton.background.states.getKey("hover").properties.fill = "#000";
      this.chartBar3.zoomOutButton.background.states.getKey("down").properties.fill = "#000";
      this.chartBar3.zoomOutButton.icon.strokeWidth = 2;


      // RESPONSIVE
      this.chartBar3.responsive.enabled = true;
      this.chartBar3.responsive.useDefault = false

      this.chartBar3.responsive.rules.push({
        relevant: function(target) {
          if (target.pixelWidth <= 500) {
            return true;
          }
          
          return false;
        },
        state: function(target, stateId) {
          
          if (target instanceof am4charts.Chart) {
            var state = target.states.create(stateId);
            state.properties.paddingTop = 10;
            state.properties.paddingRight = 0;
            state.properties.paddingBottom = 0;
            state.properties.paddingLeft = 0;
            return state;
          }
          
          if (target instanceof am4charts.Legend) {
            state = target.states.create(stateId);
            state.properties.paddingTop = 0;
            state.properties.paddingRight = 0;
            state.properties.paddingBottom = 0;
            state.properties.paddingLeft = 0;
            state.properties.marginLeft = 0;
            return state;
          }
          
          if (target instanceof am4charts.AxisRendererY) {
            state = target.states.create(stateId);
            //state.properties.inside = true;
            state.properties.maxLabelPosition = 1;
            return state;
          }
          
          if ((target instanceof am4charts.AxisLabel) && (target.parent instanceof am4charts.AxisRendererY)) { 
            state = target.states.create(stateId);
            state.properties.dy = 0;
            state.properties.paddingTop = 0;
            state.properties.paddingRight = 0;
            state.properties.paddingBottom = 0;
            state.properties.paddingLeft = 0;
            
            return state;
          }
          
          return null;
        }
      });
      //Last
    this.chartBar4 = am4core.create("barchartdiv4", am4charts.XYChart);
      if (this.chartBar4.logo) {
        this.chartBar4.logo.dispose()
      }
      this.chartBar4.background.fill = am4core.color("#0f0f18");
      this.chartBar4.background.fillOpacity = 1;
      this.chartBar4.data = [{
        "date": "",
        "balanceFinalCR": 0,
        "balanceFinalSR": 0,
        "dividendosCR": 0,
        "dividendosSR": 0,
      }];
      // AXIS
    var categoryAxis4 = this.chartBar4.xAxes.push(new am4charts.CategoryAxis());
    categoryAxis4.dataFields.category = "date";
    categoryAxis4.renderer.grid.template.disabled = true;
    categoryAxis4.renderer.minGridDistance = 50;
    categoryAxis4.renderer.labels.template.fill = am4core.color("#6b6b80");

    var axisTooltip4 = categoryAxis4.tooltip;
    axisTooltip4.background.fill = am4core.color("#000");
    axisTooltip4.background.strokeWidth = 0;
    axisTooltip4.background.cornerRadius = 5;
    axisTooltip4.background.pointerLength = 0;
    axisTooltip4.dy = 5;
    axisTooltip4.animationDuration = 0;

    var valueAxis4 = this.chartBar4.yAxes.push(new am4charts.ValueAxis());
    valueAxis4.numberFormatter.numberFormat = "'$'#,###.##a|'-$'#,###.##as";
    valueAxis4.tooltip.disabled = true;
    valueAxis4.renderer.labels.template.fill = am4core.color("#6b6b80");
    valueAxis4.renderer.grid.template.stroke = am4core.color("#ffffff");
    valueAxis4.renderer.grid.template.strokeOpacity = 0.05;
    valueAxis4.numberFormatter.bigNumberPrefixes = [
      // { "number": 1e+3, "suffix": "K" },
      // { "number": 1e+6, "suffix": "M" },
      { "number": 1e+9, "suffix": "B" },
      { "number": 1e+12, "suffix": "T" }
    ];
    valueAxis4.numberFormatter.bigNumberThreshold = "1e+3";
    valueAxis4.numberFormatter.smallNumberThreshold = "0.00";
    valueAxis4.numberFormatter.smallNumberPrefixes = [
      { "number": 1e-12, "suffix": "" },
      { "number": 1e-9, "suffix": "" },
      { "number": 1e-6, "suffix": "" },
      { "number": 1e-3, "suffix": "" },
      { "number": 1e-1, "suffix": "" },
      { "number": 0, "suffix": "" },
      { "number": 1, "suffix": "" },
    ];

    // COLORES
    this.chartBar4.colors.list = [
      am4core.color("#7c6af7"),
      am4core.color("#4ed8c0"),
      am4core.color("#f06fba"),
      am4core.color("#38bdf8"),
    ];



    // BALANCE FINAL
    var seriesD = this.chartBar4.series.push(new am4charts.LineSeries());
    seriesD.dataFields.valueY = "balanceFinalCR";
    seriesD.dataFields.categoryX = "date";
    seriesD.name = "Balance con\nReinversión";
    seriesD.tooltip.animationDuration = 0;
    seriesD.strokeWidth = 3;
    seriesD.tooltipText = "[#fff]Balance con\nReinversión:\n[bold, #fff, font-size:20px]${valueY.formatNumber('#,###.##')}[/]";
    seriesD.tooltip.background.filters.clear();
    seriesD.fillOpacity = 1;

    bullet = seriesD.bullets.push(new am4charts.CircleBullet());
    bullet.circle.fill = am4core.color("#fff");
    bullet.circle.strokeWidth = 3;

    // BALANCE FINAL SIN REINVERSION
    var series1D = this.chartBar4.series.push(new am4charts.LineSeries());
    series1D.dataFields.valueY = "balanceFinalSR";
    series1D.dataFields.categoryX = "date";
    series1D.name = "Balance sin\nReinversión";
    series1D.tooltip.animationDuration = 0;
    series1D.strokeWidth = 3;
    series1D.tooltipText = "[#000]Balance sin\nReinversión:\n[bold, #000, font-size:20px]${valueY.formatNumber('#,###.##')}[/]";
    series1D.tooltip.background.filters.clear();
    series1D.fillOpacity = 1;

    bullet = series1D.bullets.push(new am4charts.CircleBullet());
    bullet.circle.fill = am4core.color("#fff");
    bullet.circle.strokeWidth = 3;

    // DIVIDENDOS
    var series2D = this.chartBar4.series.push(new am4charts.LineSeries());
    series2D.dataFields.valueY = "dividendosCR";
    series2D.dataFields.categoryX = "date";
    series2D.name = "Dividendos con\nReinversión";
    series2D.tooltip.animationDuration = 0;
    series2D.strokeWidth = 3;
    series2D.tooltipText = "[#fff]Dividendos con\nReinversión:\n[bold, #fff, font-size:20px]${valueY.formatNumber('#,###.##')}[/]"
    series2D.tooltip.background.filters.clear();
    
    // DIVIDENDOS SIN REINVERSION
    var series3D = this.chartBar4.series.push(new am4charts.LineSeries());
    series3D.dataFields.valueY = "dividendosSR";
    series3D.dataFields.categoryX = "date";
    series3D.name = "Dividendos sin\nReinversión";
    series3D.tooltip.animationDuration = 0;
    series3D.strokeWidth = 3;
    series3D.tooltipText = "[#fff]Dividendos sin\nReinversión:\n[bold, #fff, font-size:20px]${valueY.formatNumber('#,###.##')}[/]";
    series3D.tooltip.background.filters.clear();

    bullet = series3D.bullets.push(new am4charts.CircleBullet());
    bullet.circle.fill = am4core.color("#fff");
    bullet.circle.strokeWidth = 3;


    bullet = series2D.bullets.push(new am4charts.CircleBullet());
    bullet.circle.fill = am4core.color("#fff");
    bullet.circle.strokeWidth = 3;

    //this.chartBar4.maskBullets = false;

    // CURSOR
    this.chartBar4.cursor = new am4charts.XYCursor();
    this.chartBar4.cursor.behavior = "none";
    this.chartBar4.cursor.lineY.disabled = true;
    this.chartBar4.cursor.fullWidthLineX = true;
    this.chartBar4.cursor.xAxis = categoryAxis4;
    this.chartBar4.cursor.lineX.strokeOpacity = 0;
    this.chartBar4.cursor.lineX.fill = "#000";
    this.chartBar4.cursor.lineX.fillOpacity = 0.1;
    //this.chartBar4.cursor.snapToSeries = [series1, series2, series3];


    // LEYENDA
    this.chartBar4.legend = new am4charts.Legend();
    this.chartBar4.legend.labels.template.fill = am4core.color("#eeeef6");

    // BOTON ZOOM-OUT
    this.chartBar4.zoomOutButton.align = "left";
    this.chartBar4.zoomOutButton.valign = "top";
    this.chartBar4.zoomOutButton.background.defaultState.properties.fill = "#000";
    this.chartBar4.zoomOutButton.background.defaultState.properties.fillOpacity = .2;
    this.chartBar4.zoomOutButton.background.states.getKey("hover").properties.fill = "#000";
    this.chartBar4.zoomOutButton.background.states.getKey("down").properties.fill = "#000";
    this.chartBar4.zoomOutButton.icon.strokeWidth = 2;



    // RESPONSIVE
    this.chartBar4.responsive.enabled = true;
    this.chartBar4.responsive.useDefault = false

    this.chartBar4.responsive.rules.push({
      relevant: function(target) {
        if (target.pixelWidth <= 500) {
          return true;
        }
        
        return false;
      },
      state: function(target, stateId) {
        
        if (target instanceof am4charts.Chart) {
          var state = target.states.create(stateId);
          state.properties.paddingTop = 10;
          state.properties.paddingRight = 0;
          state.properties.paddingBottom = 0;
          state.properties.paddingLeft = 0;
          return state;
        }
        
        if (target instanceof am4charts.Legend) {
          state = target.states.create(stateId);
          state.properties.paddingTop = 0;
          state.properties.paddingRight = 0;
          state.properties.paddingBottom = 0;
          state.properties.paddingLeft = 0;
          state.properties.marginLeft = 0;
          return state;
        }
        
        if (target instanceof am4charts.AxisRendererY) {
          state = target.states.create(stateId);
          //state.properties.inside = true;
          state.properties.maxLabelPosition = 1;
          return state;
        }
        
        if ((target instanceof am4charts.AxisLabel) && (target.parent instanceof am4charts.AxisRendererY)) { 
          state = target.states.create(stateId);
          state.properties.dy = 0;
          state.properties.paddingTop = 0;
          state.properties.paddingRight = 0;
          state.properties.paddingBottom = 0;
          state.properties.paddingLeft = 0;
          
          return state;
        }
        
        return null;
      }
    });

  },
}
</script>

<style>
  :root {
    --bg: #08080e;
    --bg2: #0f0f18;
    --bg3: #14141e;
    --border: rgba(255,255,255,0.07);
    --border-bright: rgba(255,255,255,0.15);
    --text: #eeeef6;
    --muted: #6b6b80;
    --accent: #7c6af7;
    --accent2: #4ed8c0;
    --accent3: #f06fba;
  }

  body { background: var(--bg) !important; }

  .v-application { background: var(--bg) !important; font-family: 'Syne', sans-serif !important; }

  input {
    font-family: 'Syne', sans-serif !important;
    color: var(--text) !important;
    background-color: transparent !important;
  }

  .v-select .v-select__selection-text { font-family: 'Syne', sans-serif !important; color: var(--text) !important; }

  .monthly-payment-mount {
    font-family: 'Raleway', sans-serif !important;
    font-weight: 800 !important;
    font-size: 2.2rem !important;
    color: var(--text) !important;
    line-height: 1.1 !important;
  }

  .monthly-payment-title {
    font-family: 'Space Mono', monospace !important;
    text-transform: uppercase !important;
    font-weight: 400 !important;
    font-size: 11px !important;
    letter-spacing: 0.12em !important;
    color: var(--muted) !important;
    margin-bottom: 1rem !important;
    margin-top: 4px !important;
  }

  .monthly-payment-line {
    width: 32px !important;
    border-width: 2px !important;
    margin-bottom: 6px !important;
    margin-top: 12px !important;
    background: linear-gradient(90deg, var(--accent), var(--accent2)) !important;
    border-color: transparent !important;
    opacity: 1 !important;
  }

  .titlepie {
    font-family: 'Raleway', sans-serif !important;
    text-transform: uppercase !important;
    font-weight: 800 !important;
    font-size: 12px !important;
    letter-spacing: 0.18em !important;
    color: var(--muted) !important;
    margin-bottom: -20px !important;
  }

  .v-card { background: var(--bg2) !important; border: 1px solid var(--border) !important; box-shadow: none !important; }

  .v-tabs { background: var(--bg3) !important; border-bottom: 1px solid var(--border) !important; }

  .v-tab {
    font-family: 'Space Mono', monospace !important;
    font-size: 11px !important;
    text-transform: uppercase !important;
    letter-spacing: 0.08em !important;
    color: var(--muted) !important;
  }

  .v-tab--selected { color: var(--accent2) !important; }

  .v-tab__slider {
    background: linear-gradient(90deg, var(--accent), var(--accent2)) !important;
    height: 2px !important;
  }

  .v-table { background: var(--bg2) !important; color: var(--text) !important; }

  .v-table thead tr th {
    font-family: 'Space Mono', monospace !important;
    font-size: 10px !important;
    text-transform: uppercase !important;
    letter-spacing: 0.08em !important;
    color: var(--muted) !important;
    background: var(--bg3) !important;
    border-bottom: 1px solid var(--border) !important;
  }

  .v-table tbody tr td {
    font-family: 'Syne', sans-serif !important;
    font-size: 13px !important;
    color: var(--text) !important;
    border-bottom: 1px solid var(--border) !important;
  }

  .v-table tbody tr:hover td { background: rgba(124,106,247,0.08) !important; }

  .v-table table tr:hover { background-color: rgba(124,106,247,0.08) !important; color: var(--text) !important; }

  .calculator-investment { line-height: 0.5em; }

  .calculator-title {
    font-family: 'Raleway', sans-serif !important;
    font-weight: 800 !important;
    font-size: 2rem !important;
    color: var(--text) !important;
    letter-spacing: 0.02em !important;
  }

  .col-style { padding-bottom: 0 !important; padding-top: 0 !important; }

  .v-field__input { max-height: 44px !important; }

  #years .v-field__field { height: 44px !important; }

  #tables { overflow-x: auto; }

  .back-btn {
    color: var(--accent2) !important;
    font-family: 'Space Mono', monospace !important;
    font-size: 11px !important;
    text-transform: uppercase !important;
    letter-spacing: 0.1em !important;
    padding-left: 0 !important;
  }
</style>