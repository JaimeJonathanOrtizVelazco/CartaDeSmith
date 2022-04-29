<template>
  <div id="app">
    <div class="d-flex flex-wrap text-center">
      <div class="col-9 col-md-9">
        <div class="mx-auto ">
          <smith-chart>
            <!-- <sm-res-circle
              v-for="circle in res"
              :res="circle.res"
              :crop="circle.crop"
              fill="rgba(255,0,255,0.5)"
              stroke-width="2"
              stroke="black"
            ></sm-res-circle>

            <sm-react-arc
              v-for="circle in react"
              :react="circle.react"
              :crop="circle.crop"
              fill="rgba(255,0,255,0.5)"
              stroke-width="2"
              stroke="black"
            ></sm-react-arc>

            <sm-vswr-circle
              :res="vswr.res"
              :react="vswr.crop"
              stroke-width="2"
              stroke="black"
            ></sm-vswr-circle> -->
            <g fill="none" stroke-width="3" :stroke="stroke">
              <circle :cx="radius" :cy="radius" :r="r" />
              <sm-point
                :res="point.res"
                :react="point.react"
                fill="blue"
              ></sm-point>
            </g>
            <sm-point-cr :h="p.h" :angle="p.angle" fill="green"></sm-point-cr>
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
        </div>
        <div class="card mb-4">
          <div class="card-header d-flex justify-content-between">
            <h4>Carga en la carta</h4>
            <button
              @click="localizarCarga"
              type="button"
              class="btn btn-primary"
            >
              Localizar
            </button>
          </div>
        </div>
        <!-- <ul class="list-group list-group-flush">
            <li v-for="(point, i) in points" class="list-group-item">
              <div class="form-inline">
                <label class="sr-only" for="inlineFormInput">Resistance</label>
                <div class="input-group mr-sm-2">
                  <div class="input-group-prepend">
                    <div class="input-group-text">Resistance</div>
                  </div>
                  <input
                    v-model.number="point.res"
                    type="number"
                    class="form-control"
                    id="inlineFormInput"
                    placeholder="Resistance"
                  />
                </div>

                <label class="sr-only" for="inlineFormInputGroup"
                  >Reactance</label
                >
                <div class="input-group">
                  <div class="input-group-prepend">
                    <div class="input-group-text">Reactance</div>
                  </div>
                  <input
                    v-model.number="point.react"
                    type="number"
                    class="form-control"
                    id="inlineFormInputGroup"
                    placeholder="Reactance"
                  />
                </div>

                <button
                  type="button"
                  class="btn btn-outline-danger ml-auto"
                  @click="points.splice(i, 1)"
                >
                  X
                </button>
              </div>
            </li>
          </ul> -->
        <div class="card mb-4">
          <div class="card-header d-flex justify-content-between">
            <h4>Coeficiente de reflexion:</h4>
            <button @click="addPoint" type="button" class="btn btn-primary">
              Calcular
            </button>
          </div>
          <p>{{ coeficiente }}</p>
        </div>
        <div class="card mb-4">
          <div class="card-header d-flex justify-content-between">
            <h4>Impedancia de entrada Z<sub>i</sub></h4>
            <button @click="impedancia" type="button" class="btn btn-primary">
              Calcular
            </button>
          </div>
          <p>{{ impedanciaEntrada }}</p>
        </div>
      </div>
    </div>
    <!-- 
    <div class="card mb-4">
      <div class="card-header d-flex justify-content-between">
        <h4>Constant Resistance Circles</h4>
        <button @click="addResCircle" type="button" class="btn btn-primary">
          Add circle
        </button>
      </div>

      <ul class="list-group list-group-flush">
        <li v-for="(circle, i) in res" class="list-group-item">
          <div class="form-inline">
            <label class="sr-only" for="inlineFormInput">Resistance</label>
            <div class="input-group mb-2">
              <div class="input-group-prepend">
                <div class="input-group-text">Resistance</div>
              </div>
              <input
                v-model.number="circle.res"
                type="number"
                class="form-control"
                id="inlineFormInput"
                placeholder="Resistance"
              />
            </div>

            <label class="sr-only" for="inlineFormInput">Crop</label>
            <div class="input-group mb-2">
              <div class="input-group-prepend">
                <div class="input-group-text">Crop</div>
              </div>
              <input
                v-model.number="circle.crop"
                type="number"
                class="form-control"
                id="inlineFormInput"
                placeholder="No"
              />
            </div>

            <button
              type="button"
              class="btn btn-outline-danger ml-auto"
              @click="res.splice(i, 1)"
            >
              X
            </button>
          </div>
        </li>
      </ul>
    </div>

    <div class="card mb-4">
      <div class="card-header d-flex justify-content-between">
        <h4>Constant Reactance Arcs</h4>
        <button @click="addReactCircle" type="button" class="btn btn-primary">
          Add arc
        </button>
      </div>

      <ul class="list-group list-group-flush">
        <li v-for="(circle, i) in react" class="list-group-item">
          <div class="form-inline">
            <label class="sr-only" for="inlineFormInput">Reactance</label>
            <div class="input-group mb-2">
              <div class="input-group-prepend">
                <div class="input-group-text">Reactance</div>
              </div>
              <input
                v-model.number="circle.react"
                type="number"
                class="form-control"
                id="inlineFormInput"
                placeholder="Reactance"
              />
            </div>

            <label class="sr-only" for="inlineFormInput">Crop</label>
            <div class="input-group mb-2">
              <div class="input-group-prepend">
                <div class="input-group-text">Crop</div>
              </div>
              <input
                v-model.number="circle.crop"
                type="number"
                class="form-control"
                id="inlineFormInput"
                placeholder="No"
              />
            </div>

            <button
              type="button"
              class="btn btn-outline-danger ml-auto"
              @click="react.splice(i, 1)"
            >
              X
            </button>
          </div>
        </li>
      </ul>
    </div>

    <div class="card mb-4">
      <div class="card-header d-flex justify-content-between">
        <h4>Constant VSWR circles</h4>
        <button @click="addVswrCircle" type="button" class="btn btn-primary">
          Add circle
        </button>
      </div>

      <ul class="list-group list-group-flush">
        <li v-for="(circle, i) in vswr" class="list-group-item">
          <div class="form-inline">
            <label class="sr-only" for="inlineFormInput">Resistance</label>
            <div class="input-group mb-2">
              <div class="input-group-prepend">
                <div class="input-group-text">Resistance</div>
              </div>
              <input
                v-model.number="circle.res"
                type="number"
                class="form-control"
                id="inlineFormInput"
                placeholder="Resistance"
              />
            </div>

            <label class="sr-only" for="inlineFormInputGroup">Reactance</label>
            <div class="input-group mb-2">
              <div class="input-group-prepend">
                <div class="input-group-text">Reactance</div>
              </div>
              <input
                v-model.number="circle.react"
                type="number"
                class="form-control"
                id="inlineFormInputGroup"
                placeholder="Reactance"
              />
            </div>

            <button
              type="button"
              class="btn btn-outline-danger ml-auto"
              @click="vswr.splice(i, 1)"
            >
              X
            </button>
          </div>
        </li>
      </ul>
    </div> -->
  </div>
</template>

<script>
import SmithChart from "./components/smith-chart.vue";
import SmPoint from "./components/sm-point.vue";
import SmResCircle from "./components/sm-res-circle.vue";
import SmReactArc from "./components/sm-react-arc.vue";
import SmVswrCircle from "./components/sm-vswr-circle.vue";
import * as math from "mathjs";
import SmPointCr from "./components/sm-point-cr.vue";

export default {
  name: "app",
  components: {
    SmithChart,
    SmPoint,
    SmResCircle,
    SmReactArc,
    SmVswrCircle,
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
    r: function() {
      let r = Math.sqrt(Math.pow(this.cx, 2) + Math.pow(this.cy, 2));
      return r;
    },
    pCoef: function() {
      let p = math.divide(
        math.complex(this.carga.re - this.impedanciaCarac, this.carga.im),
        math.complex(this.carga.re + this.impedanciaCarac, this.carga.im)
      );
      return p;
    },
    carga: function() {
      return math.complex(this.impedanciaCarga);
    },
    lambda: function() {
      return 3e8 / this.frecuencia;
    },
  },
  data: function() {
    return {
      impedanciaCarac: 0,
      impedanciaCarga: 0,
      impedanciaEntrada: "",
      point: { res: 1, react: 0 },
      p: { h: 1, angle: 0 },
      l: 0,
      frecuencia: 0,
      er: 1,
      beta: 0,
      coeficiente: "",
      ref: {},
      res: [],
      react: [],
      vswr: [],
      radius: 400,
    };
  },
  props: {
    stroke: {
      default: "blue",
    },
  },
  methods: {
    addPoint() {
      this.p.h = math.abs(this.pCoef);
      this.p.angle = (math.atan(this.pCoef.im / this.pCoef.re) * 180) / math.pi;
      this.coeficiente = this.p.h + "∠" + this.p.angle;
    },
    localizarCarga() {
      let impedance = math.divide(this.carga, this.impedanciaCarac);
      this.point.res = impedance.re;
      this.point.react = impedance.im;
    },
    impedancia() {
      this.beta = (2 * math.pi * math.sqrt(this.er)) / this.lambda;
      if (this.carga.re == 0 && this.carga.im == 0) {
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
        this.impedanciaEntrada = math.multiply(
          this.impedanciaCarac,
          math.divide(
            math.add(
              1,
              math.multiply(
                this.pCoef,
                math.exp(math.complex(0, -2 * this.beta * this.l))
              )
            ),
            math.subtract(
              1,
              math.multiply(
                this.pCoef,
                math.exp(math.complex(0, -2 * this.beta * this.l))
              )
            )
          )
        );
      }
    },
    // addResCircle() {
    //   this.res.push({ res: 1, crop: 0 });
    // },
    // addReactCircle() {
    //   this.react.push({ react: 1, crop: "" });
    // },
    // addVswrCircle() {
    //   this.vswr.push({ res: 1, react: "" });
    // },
  },
};
</script>
