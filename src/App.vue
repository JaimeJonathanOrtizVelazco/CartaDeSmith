<template>
  <div id="app">
    <div class="d-flex flex-wrap text-center">
      <div class="col-9 col-md-9">
        <div class="mx-auto ">
          <smith-chart>
            <sm-res-circle
              :res="1"
              fill="rgba(0,0,255,0.5)"
              stroke="orange"
              stroke-width="2"
            ></sm-res-circle>
            <!-- Carga -->
            <g fill="none" stroke-width="3" :stroke="stroke">
              <circle :cx="radius" :cy="radius" :r="r" />
              <sm-point
                :res="point.res"
                :react="point.react"
                fill="blue"
              ></sm-point>
              <g stroke="blue">
                <line
                  :x1="radius"
                  :y1="radius"
                  :x2="getXPoint"
                  :y2="getYPoint"
                />
              </g>
            </g>
            <!-- Coeficiente de reflexion -->
            <sm-point-cr :h="p.h" :angle="p.angle" fill="green"></sm-point-cr>
            <g stroke="green" stroke-width="3">
              <line
                :x1="radius"
                :y1="radius"
                :x2="getXCoePoint"
                :y2="getYCoePoint"
              />
            </g>
            <!-- Impedancia de entrada -->
            <sm-point
              :res="imped.res"
              :react="imped.react"
              fill="red"
            ></sm-point>
            <g stroke="red" stroke-width="3">
              <line
                :x1="radius"
                :y1="radius"
                :x2="getXImpedPoint"
                :y2="getYImpedPoint"
              />
            </g>
            <!-- Admitancia -->

            <sm-point
              :res="pointAdmitancia.res"
              :react="pointAdmitancia.react"
              fill="purple"
            ></sm-point>
            <g stroke="purple">
              <line
                :x1="radius"
                :y1="radius"
                :x2="getXAdmitancia"
                :y2="getYAdmitancia"
              />
            </g>
          </smith-chart>
        </div>
      </div>
      <div class="col-3 col-md-3">
        <div class="card mb-4">
          <div class="card-header d-flex justify-content-between">
            <h4>Datos de la linea de transmision</h4>
          </div>
          <div class="input-group mr-sm-2">
            <div class="input-group-prepend">
              <div class="input-group-text">Impedancia caracteristica</div>
            </div>
            <input
              v-model.number="impedanciaCarac"
              type="number"
              class="form-control"
              id="inlineFormInput"
              placeholder="0"
            />
          </div>
          <div class="input-group mr-sm-2">
            <div class="input-group-prepend">
              <div class="input-group-text">Impedancia de la carga</div>
            </div>
            <input
              v-model="impedanciaCarga"
              type="text"
              class="form-control"
              id="inlineFormInput"
              placeholder="0"
            />
          </div>
          <div class="input-group mr-sm-2">
            <div class="input-group-prepend">
              <div class="input-group-text">Impedancia de entrada</div>
            </div>
            <input
              v-model="impedanciaEntrada"
              type="text"
              class="form-control"
              id="inlineFormInput"
              placeholder="0"
            />
          </div>
          <div class="input-group mr-sm-2">
            <div class="input-group-prepend">
              <div class="input-group-text">Longitud de la linea</div>
            </div>
            <input
              v-model.number="l"
              type="number"
              class="form-control"
              id="inlineFormInput"
              placeholder="0"
            />
          </div>
          <div class="input-group mr-sm-2">
            <div class="input-group-prepend">
              <div class="input-group-text">Frecuencia</div>
            </div>
            <input
              v-model.number="frecuencia"
              type="number"
              class="form-control"
              id="inlineFormInput"
              placeholder="0"
            />
          </div>
          <div class="input-group mr-sm-2">
            <div class="input-group-prepend">
              <div class="input-group-text">Er</div>
            </div>
            <input
              v-model.number="er"
              type="number"
              class="form-control"
              id="inlineFormInput"
              placeholder="0"
            />
          </div>
          <button @click="todo" type="button" class="btn btn-primary">
            Calcular todo
          </button>
        </div>
        <div class="card mb-4">
          <div class="card-header d-flex justify-content-between">
            <div :style="[circleRepre, { 'background-color': 'blue' }]"></div>
            <h4>Localizar carga</h4>
            <button
              @click="localizarCarga"
              type="button"
              class="btn btn-primary"
            >
              Localizar
            </button>
          </div>
        </div>
        <div class="card mb-4">
          <div class="card-header d-flex justify-content-between">
            <div :style="[circleRepre, { 'background-color': 'green' }]"></div>
            <h4>Coeficiente de reflexion</h4>
            <button @click="addPoint" type="button" class="btn btn-primary">
              Calcular
            </button>
          </div>
          <p>{{ coeficiente }}</p>
        </div>
        <div class="card mb-4">
          <div class="card-header d-flex justify-content-between">
            <div :style="[circleRepre, { 'background-color': 'red' }]"></div>
            <h4>Impedancia de entrada Z<sub>i</sub></h4>
            <button @click="impedancia" type="button" class="btn btn-primary">
              {{ accionImpedancia }}
            </button>
          </div>
          <p>{{ impedanciaEntrada }}</p>
        </div>
        <div class="card mb-4">
          <div class="card-header d-flex justify-content-between">
            <!-- <div :style="[circleRepre, { 'background-color': 'red' }]"></div> -->
            <h4>Longitud de la linea</h4>
            <button
              @click="longitudLinea"
              type="button"
              class="btn btn-primary"
            >
              Calcular
            </button>
          </div>
          <p>{{ longLinea }}</p>
        </div>
        <div class="card mb-4">
          <div class="card-header d-flex justify-content-between">
            <h4>Acomplador Stub</h4>
            <button @click="stub" type="button" class="btn btn-primary">
              Calcular
            </button>
          </div>
          <div class="d-flex align-items-center">
            <div :style="[circleRepre, { 'background-color': 'purple' }]"></div>
            Admitancia
          </div>

          <p>Longitud: {{ longitudStub }}</p>
          <p>Distancia a la carga: {{ distanciaStubCarga }}</p>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import SmithChart from "./components/smith-chart.vue";
import SmPoint from "./components/sm-point.vue";
import * as math from "mathjs";
import SmPointCr from "./components/sm-point-cr.vue";

export default {
  name: "app",
  components: {
    SmithChart,
    SmPoint,
    SmPointCr,
  },
  computed: {
    cx: function() {
      let r = this.point.res;
      let x = this.point.react;
      let a =
        (Math.pow(r, 2) - 1 + Math.pow(x, 2)) /
        (Math.pow(r + 1, 2) + Math.pow(x, 2));

      return a * this.radius;
    },
    cy: function() {
      let r = this.point.res;
      let x = this.point.react;

      let b = (2 * x) / (Math.pow(r + 1, 2) + Math.pow(x, 2));

      return -b * this.radius;
    },
    cxImped: function() {
      let r = this.imped.res;
      let x = this.imped.react;
      let a =
        (Math.pow(r, 2) - 1 + Math.pow(x, 2)) /
        (Math.pow(r + 1, 2) + Math.pow(x, 2));

      return a * this.radius;
    },
    cyImped: function() {
      let r = this.imped.res;
      let x = this.imped.react;

      let b = (2 * x) / (Math.pow(r + 1, 2) + Math.pow(x, 2));

      return -b * this.radius;
    },
    cxAdmitancia: function() {
      let r = this.pointAdmitancia.res;
      let x = this.pointAdmitancia.react;
      let a =
        (Math.pow(r, 2) - 1 + Math.pow(x, 2)) /
        (Math.pow(r + 1, 2) + Math.pow(x, 2));

      return a * this.radius;
    },
    cyAdmitancia: function() {
      let r = this.pointAdmitancia.res;
      let x = this.pointAdmitancia.react;

      let b = (2 * x) / (Math.pow(r + 1, 2) + Math.pow(x, 2));

      return -b * this.radius;
    },
    ca: function() {
      return this.p.h * this.radius * math.cos((this.p.angle / 180) * math.pi);
    },
    co: function() {
      return this.p.h * this.radius * math.sin((this.p.angle / 180) * math.pi);
    },
    getXPoint: function() {
      let rad = this.rad(this.cx, this.cy);
      return 500 * (this.cx / rad) + this.radius;
    },
    getYPoint: function() {
      let rad = this.rad(this.cx, this.cy);
      return 500 * (this.cy / rad) + this.radius;
    },
    getXCoePoint: function() {
      let rad = this.rad(this.ca, this.co);
      return 500 * (this.ca / rad) + this.radius;
    },
    getYCoePoint: function() {
      let rad = this.rad(this.ca, this.co);
      return 500 * (-this.co / rad) + this.radius;
    },
    getXImpedPoint: function() {
      let rad = this.rad(this.cxImped, this.cyImped);
      return 500 * (this.cxImped / rad) + this.radius;
    },
    getYImpedPoint: function() {
      let rad = this.rad(this.cxImped, this.cyImped);
      return 500 * (this.cyImped / rad) + this.radius;
    },
    getXAdmitancia: function() {
      let rad = this.rad(this.cxAdmitancia, this.cyAdmitancia);
      return 500 * (this.cxAdmitancia / rad) + this.radius;
    },
    getYAdmitancia: function() {
      let rad = this.rad(this.cxAdmitancia, this.cyAdmitancia);
      return 500 * (this.cyAdmitancia / rad) + this.radius;
    },
    r: function() {
      let r = Math.sqrt(Math.pow(this.cx, 2) + Math.pow(this.cy, 2));
      return r;
    },
    rAdmitancia: function() {
      let r = Math.sqrt(
        Math.pow(this.cxAdmitancia, 2) + Math.pow(this.cyAdmitancia, 2)
      );
      return r;
    },
    accionImpedancia: function() {
      if (this.impedanciaEntrada == 0) {
        return "Calcular";
      } else {
        return "Localizar";
      }
    },
  },
  data: function() {
    return {
      impedanciaCarac: 0,
      impedanciaCarga: 0,
      impedanciaEntrada: "",
      point: { res: 1, react: 0 },
      imped: { res: 1, react: 0 },
      pointAdmitancia: { res: 1, react: 0 },
      p: { h: 1, angle: 0 },
      l: 0,
      angulocarga: 0,
      anguloimpentrada: 0,
      frecuencia: 0,
      longLinea: "",
      longitudStub: "",
      distanciaStubCarga: "",
      er: 1,
      beta: 0,
      coeficiente: "",
      radius: 400,

      circleRepre: {
        width: "10px",
        height: "10px",
        "border-radius": "50%",
      },
    };
  },
  props: {
    stroke: {
      default: "blue",
    },
  },
  methods: {
    todo() {
      this.addPoint();
      this.localizarCarga();
      this.impedancia();
      this.longitudLinea();
    },
    addPoint() {
      let coef = this.pCoef();
      this.p.h = math.abs(coef);
      this.p.angle =
        (math.atan(coef.im / (coef.re == 0 ? 1 : coef.re)) * 180) / math.pi;
      this.coeficiente = this.p.h + "∠" + this.p.angle;
    },
    localizarCarga() {
      let impedance = math.divide(this.carga(), this.impedanciaCarac);
      this.point.res = impedance.re;
      this.point.react = impedance.im;
    },
    impedancia() {
      if (this.impedanciaEntrada == "") {
        this.beta = (2 * math.pi * math.sqrt(this.er)) / this.lambdaCero();
        let carga = this.carga();
        if (carga.re == 0 && carga.im == 0) {
          if (math.abs(this.beta * this.l) * 10 < 1) {
            this.impedanciaEntrada = math.complex(
              0,
              this.impedanciaCarac * this.beta * this.l
            );
          } else {
            this.impedanciaEntrada = math.multiply(
              this.impedanciaCarac,
              math.divide(
                math.subtract(
                  1,
                  math.exp(math.complex(0, -2 * this.beta * this.l))
                ),
                math.add(1, math.exp(math.complex(0, -2 * this.beta * this.l)))
              )
            );
          }
        } else if (this.p.h >= 0.99 && this.p.angle <= 0.1) {
          if (math.abs(this.beta * this.l) * 10 < 1) {
            this.impedanciaEntrada = math.divide(
              this.impedanciaCarac,
              math.complex(0, this.beta * this.l)
            );
          } else {
            this.impedanciaEntrada = math.multiply(
              this.impedanciaCarac,
              math.divide(
                math.add(1, math.exp(math.complex(0, -2 * this.beta * this.l))),
                math.subtract(
                  1,
                  math.exp(math.complex(0, -2 * this.beta * this.l))
                )
              )
            );
          }
        } else {
          let coef = this.pCoef();
          this.impedanciaEntrada = math.multiply(
            this.impedanciaCarac,
            math.divide(
              math.add(
                1,
                math.multiply(
                  coef,
                  math.exp(math.complex(0, -2 * this.beta * this.l))
                )
              ),
              math.subtract(
                1,
                math.multiply(
                  coef,
                  math.exp(math.complex(0, -2 * this.beta * this.l))
                )
              )
            )
          );
        }
      } else {
        this.impedanciaEntrada = math.complex(this.impedanciaEntrada);
      }
      let impedCalc = math.divide(this.impedanciaEntrada, this.impedanciaCarac);
      this.imped.res = impedCalc.re;
      this.imped.react = impedCalc.im;
    },
    longitudLinea() {
      let angulocarga =
        ((math.acos(this.cx / this.rad(this.cx, this.cy)) * 180) / math.pi) *
        (-math.abs(this.cy) / this.cy);
      this.angulocarga = angulocarga;
      let anguloimpentrada =
        ((math.acos(this.cxImped / this.rad(this.cxImped, this.cyImped)) *
          180) /
          math.pi) *
        (-math.abs(this.cyImped) / this.cyImped);
      this.anguloimpentrada = anguloimpentrada;
      let angulolinea = 0;
      if (angulocarga < 0) {
        if (anguloimpentrada > angulocarga) {
          angulolinea = 360 + angulocarga - anguloimpentrada;
        } else {
          angulolinea = angulocarga - anguloimpentrada;
        }
      } else {
        if (angulocarga > anguloimpentrada) {
          angulolinea = angulocarga - anguloimpentrada;
        } else {
          angulolinea = 360 + angulocarga - anguloimpentrada;
        }
      }
      if (this.frecuencia > 0) {
        this.longLinea =
          "" +
          ((angulolinea * 0.5) / 360) *
            (this.lambdaCero() / math.sqrt(this.er)) +
          " m";
      } else {
        this.longLinea = "" + (angulolinea * 0.5) / 360 + " λ";
      }
    },
    stub() {
      let yl = math.divide(1, math.divide(this.carga(), this.impedanciaCarac));
      this.pointAdmitancia.res = yl.re;
      this.pointAdmitancia.react = yl.im;
    },
    pCoef() {
      let carga = this.carga();
      let p = math.divide(
        math.complex(carga.re - this.impedanciaCarac, carga.im),
        math.complex(carga.re + this.impedanciaCarac, carga.im)
      );
      return p;
    },
    carga() {
      return math.complex(this.impedanciaCarga);
    },
    lambdaCero() {
      return 3e8 / this.frecuencia;
    },
    rad(cx, cy) {
      return math.sqrt(cx * cx + cy * cy) + 1;
    },
  },
};
</script>
