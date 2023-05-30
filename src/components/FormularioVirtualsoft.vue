<template>
  <div class="bg-gray-100 min-h-screen">
    <div class="container mx-auto py-12">
      <div class="max-w-5xl mx-auto bg-white rounded-lg shadow-md">
        <div class="py-6 px-8">
          <div class="flex items-center justify-center mb-6">
            <img src="../assets/vue.png" alt="Logo de Vue.js" class="h-10" />
            <img
              src="../assets/tailwind.png"
              alt="Logo de Tailwind CSS"
              class="h-10"
            />
          </div>
          <progress
            class="w-full h-2 rounded-lg bg-gray-300 mb-6"
            :value="currentStep"
            :max="totalSteps"
          ></progress>

          <div v-show="currentStep === 1">
            <!-- Paso 1 -->
            <h3 class="text-xl font-bold mb-4">Paso 1: Información personal</h3>
            <div class="flex flex-row justify-stretch">
              <div class="flex-grow">
                <!-- País -->
                <div class="mb-4 flex items-center text-start">
                  <label class="block font-bold text-cyan-600 mr-2 text-sm"
                    >País</label
                  >
                  <select
                    :class="{
                      'border-red-500': !validFields.selectedCountry,
                      'border-green-500': validFields.selectedCountry,
                    }"
                    class="border rounded-md px-4 py-2 focus:outline-none mx-2"
                    v-model="validFields.selectedCountry"
                    @change="handleCountryChange"
                  >
                    <option disabled value="">Selecciona...</option>
                    <option
                      v-for="country in validFields.countries"
                      :value="country.name"
                      :key="country.alpha2Code"
                    >
                      {{ country.name }}
                    </option>
                  </select>
                  <span
                    v-show="!validFields.selectedCountry"
                    class="ml-2 mr-1 text-red-500"
                    >X</span
                  >
                  <span
                    v-show="validFields.selectedCountry"
                    class="ml-2 mr-1 text-green-500"
                    >✓</span
                  >
                </div>

                <!-- Género -->
                <div class="mb-4 flex items-center text-start">
                  <label class="block font-bold text-cyan-600 mr-2 text-sm"
                    >Género</label
                  >
                  <select
                    v-model="validFields.genero"
                    :class="{
                      'border-red-500': !validFields.genero,
                      'border-green-500': validFields.genero,
                    }"
                    class="border rounded-md px-4 py-2 focus:outline-none mx-2"
                  >
                    <option value="">Selecciona...</option>
                    <option value="masculino">Masculino</option>
                    <option value="femenino">Femenino</option>
                  </select>
                  <span v-show="!validFields.genero" class="ml-2 text-red-500"
                    >X</span
                  >
                  <span v-show="validFields.genero" class="ml-2 text-green-500"
                    >✓</span
                  >
                </div>

                <!-- Primer nombre -->
                <div class="mb-4 flex items-center text-start mx-0">
                  <label class="block font-bold text-cyan-600 mr-2 text-sm"
                    >Primer nombre</label
                  >
                  <input
                    type="text"
                    v-model="validFields.primerNombre"
                    :class="{
                      'border-red-500': !validFields.primerNombre,
                      'border-green-500': validFields.primerNombre,
                    }"
                    class="border rounded-md px-4 py-2 focus:outline-none mx-2"
                    @input="validateField(validFields.primerNombre)"
                  />
                  <span
                    v-show="!validFields.primerNombre"
                    class="ml-2 text-red-500"
                    >X</span
                  >
                  <span
                    v-show="validFields.primerNombre"
                    class="ml-2 text-green-500"
                    >✓</span
                  >
                </div>

                <!-- Segundo nombre -->
                <div class="mb-4 flex items-center text-start mx-0">
                  <label class="block font-bold text-cyan-600 mr-2 text-sm"
                    >Segundo nombre</label
                  >
                  <input
                    type="text"
                    v-model="validFields.segundoNombre"
                    :class="{
                      'border-red-500': !validFields.segundoNombre,
                      'border-green-500': validFields.segundoNombre,
                    }"
                    class="border rounded-md px-4 py-2 focus:outline-none mx-2"
                    @input="validateField(validFields.segundoNombre)"
                  />
                  <span
                    v-show="!validFields.segundoNombre"
                    class="ml-2 text-red-500"
                    >X</span
                  >
                  <span
                    v-show="validFields.segundoNombre"
                    class="ml-2 text-green-500"
                    >✓</span
                  >
                </div>

                <!-- Fecha de nacimiento -->
                <div class="mb-4 flex items-center text-start mx-0">
                  <label class="block font-bold text-cyan-600 mr-2 text-sm"
                    >Fecha de nacimiento</label
                  >
                  <div class="relative">
                    <input
                      type="date"
                      v-model="validFields.fechaNacimiento"
                      :class="{
                        'border border-red-500':
                          !validFields.fechaNacimiento ||
                          !validarEdad(validFields.fechaNacimiento),
                        'border border-green-500':
                          validFields.fechaNacimiento &&
                          validarEdad(validFields.fechaNacimiento),
                      }"
                      class="border rounded-md px-4 py-2 focus:outline-none mx-2 inputdate"
                      @input="validateField(validFields.fechaNacimiento)"
                    />
                    <p
                      v-if="!validarEdad(validFields.fechaNacimiento)"
                      style="color: red"
                      class="absolute left-3 top-19"
                    >
                      Debes tener al menos 18 años.
                    </p>
                  </div>

                  <p
                    v-show="
                      !validFields.fechaNacimiento ||
                      !validarEdad(validFields.fechaNacimiento)
                    "
                    class="text-red-500 ml-2"
                  >
                    X
                  </p>
                  <p
                    v-show="
                      validFields.fechaNacimiento &&
                      validarEdad(validFields.fechaNacimiento)
                    "
                    class="text-green-500 ml-2"
                  >
                    ✓
                  </p>
                </div>
              </div>

              <div class="flex-grow">
                <!-- Tipo de documento -->
                <div class="mb-4 flex items-center text-start mx-0">
                  <label class="block font-bold text-cyan-600 mr-2 text-sm"
                    >Tipo de documento</label
                  >
                  <select
                    v-model="validFields.tipoDocumento"
                    :class="{
                      'border border-red-500': !validFields.tipoDocumento,
                      'border border-green-500': validFields.tipoDocumento,
                    }"
                    class="border rounded-md px-4 py-2 focus:outline-none mx-2"
                  >
                    <option value="">Selecciona...</option>
                    <option value="cc">Cédula de ciudadanía</option>
                    <option value="pasaporte">Pasaporte</option>
                    <option value="ce">Cédula de extranjería</option>
                  </select>
                  <span
                    v-show="!validFields.tipoDocumento"
                    class="ml-2 text-red-500"
                    >X</span
                  >
                  <span
                    v-show="validFields.tipoDocumento"
                    class="ml-2 text-green-500"
                    >✓</span
                  >
                </div>

                <!-- Número de documento -->
                <div class="mb-4 flex items-center text-start mx-0">
                  <label class="block font-bold text-cyan-600 mr-2 text-sm"
                    >Numero de documento</label
                  >
                  <input
                    type="number"
                    v-model="validFields.numeroDocumento"
                    :class="{
                      'border-red-500': validFields.numeroDocumento.length < 5,
                      'border-green-500':
                        validFields.numeroDocumento &&
                        validFields.numeroDocumento.length >= 5,
                    }"
                    class="border rounded-md px-4 py-2 focus:outline-none mx-2"
                    @input="validateField(validFields.numeroDocumento)"
                    pattern="[0-9]+"
                    inputmode="numeric"
                  />
                  <span
                    v-show="validFields.numeroDocumento.length < 5"
                    class="ml-2 text-red-500"
                    >X</span
                  >
                  <span
                    v-show="
                      validFields.numeroDocumento &&
                      validFields.numeroDocumento.length >= 5
                    "
                    class="ml-2 text-green-500"
                    >✓</span
                  >
                </div>

                <!-- Documento Frente -->
                <div class="mb-4 flex items-center text-start mx-0">
                  <label class="block font-bold text-cyan-600 mr-2 text-sm"
                    >Documento - Frente</label
                  >
                  <input
                    type="file"
                    :id="validFields.fotoFrente"
                    @change="handleFrenteUpload"
                    accept="image/jpeg"
                    :class="{
                      'border-red-500': !validFields.fotoFrente,
                      'border-green-500': validFields.fotoFrente,
                    }"
                    class="border rounded-md px-4 py-2 focus:outline-none mx-2"
                  />
                  <span
                    v-show="!validFields.fotoFrente"
                    class="ml-2 text-red-500"
                    >X</span
                  >
                  <span
                    v-show="validFields.fotoFrente"
                    class="ml-2 text-green-500"
                    >✓</span
                  >
                </div>

                <!-- Documento Reverso -->
                <div class="mb-4 flex items-center text-start mx-0">
                  <label class="block font-bold text-cyan-600 mr-2 text-sm"
                    >Documento - Reverso</label
                  >
                  <input
                    type="file"
                    :id="validFields.fotoReverso"
                    @change="handleReversoUpload"
                    accept="image/jpeg"
                    :class="{
                      'border-red-500': !validFields.fotoReverso,
                      'border-green-500': validFields.fotoReverso,
                    }"
                    class="border rounded-md px-4 py-2 focus:outline-none mx-2"
                  />
                  <span
                    v-show="!validFields.fotoReverso"
                    class="ml-2 text-red-500"
                    >X</span
                  >
                  <span
                    v-show="validFields.fotoReverso"
                    class="ml-2 text-green-500"
                    >✓</span
                  >
                </div>
              </div>
            </div>

            <!-- Avanzar -->
            <div class="flex justify-end">
              <button
                type="button"
                class="px-4 py-2 bg-green-500 text-white rounded-lg mt-3"
                :class="{ 'btn-opaco': !isPaso1Valido }"
                :disabled="!isPaso1Valido"
                @click="nextStep"
              >
                Avanzar
              </button>
            </div>
          </div>

          <div v-show="currentStep === 2">
            <!-- Paso 2 -->
            <h3 class="text-xl font-bold mb-4">
              Paso 2: Información de contacto
            </h3>

            <!-- Correo electrónico -->
            <div class="mb-4 flex items-center text-start">
              <label class="block font-bold text-cyan-600 mr-2 text-sm"
                >Correo electrónico</label
              >
              <input
                type="email"
                v-model="validFields.correoElectronico"
                :class="{
                  'border-red-500':
                    !validFields.correoElectronico ||
                    !validarCorreoElectronico(validFields.correoElectronico),
                  'border-green-500': validFields.correoElectronico,
                }"
                class="border rounded-md px-4 py-2 focus:outline-none mx-2"
                @input="validateField(validFields.correoElectronico)"
              />
              <span
                v-show="
                  !validFields.correoElectronico ||
                  !validarCorreoElectronico(validFields.correoElectronico)
                "
                class="ml-2 text-red-500"
                >X</span
              >
              <span
                v-show="
                  validFields.correoElectronico &&
                  validarCorreoElectronico(validFields.correoElectronico)
                "
                class="ml-2 text-green-500"
                >✓</span
              >
              <span
                v-show="
                  !validFields.correoElectronico ||
                  !validarCorreoElectronico(validFields.correoElectronico)
                "
                class="text-red-500"
                >Ingrese un correo electrónico válido</span
              >
            </div>

            <div class="flex justify-start mx-0">
              <!-- Contraseña -->
              <div class="mb-4 flex items-center text-start">
                <label class="block font-bold text-cyan-600 mr-2 text-sm"
                  >Contraseña</label
                >
                <input
                  type="password"
                  v-model="validFields.contrasena"
                  :class="{
                    'border-red-500': !validFields.contrasena,
                    'border-green-500': validFields.contrasena,
                  }"
                  class="border rounded-md px-4 py-2 focus:outline-none mx-2"
                  @input="validateField(validFields.contrasena)"
                />
                <span v-show="!validFields.contrasena" class="ml-2 text-red-500"
                  >X</span
                >
                <span
                  v-show="validFields.contrasena"
                  class="ml-2 text-green-500"
                  >✓</span
                >
              </div>

              <!-- Confirmación de Contraseña -->
              <div class="mb-4 flex items-center text-start">
                <label class="block font-bold text-cyan-600 mr-2 text-sm"
                  >Confirmación de Contraseña</label
                >
                <div class="relative">
                  <input
                    type="password"
                    v-model="validFields.confirmarContrasena"
                    :class="{
                      'border-red-500': !validFields.confirmarContrasena,
                      'border-green-500': validFields.confirmarContrasena,
                    }"
                    class="border rounded-md px-4 py-2 focus:outline-none mx-2"
                    @input="validateField(validFields.confirmarContrasena)"
                  />
                  <p
                    v-show="
                      validFields.contrasena !== validFields.confirmarContrasena
                    "
                    class="text-red-500 absolute left-3 top-19"
                  >
                    Las contraseñas no coinciden
                  </p>
                </div>
                <p
                  v-show="!validFields.confirmarContrasena"
                  class="ml-2 text-red-500"
                >
                  X
                </p>
                <p
                  v-show="validFields.confirmarContrasena"
                  class="ml-2 text-green-500"
                >
                  ✓
                </p>
              </div>
            </div>

            <div class="flex justify-start">
              <!-- Numero de teléfono -->
              <div class="mb-4 flex items-center text-start">
                <label class="block font-bold text-cyan-600 mr-2 text-sm"
                  >Número de teléfono</label
                >
                <input
                  type="text"
                  v-model="validFields.numeroTelefono"
                  :class="{
                    'border-red-500': !validFields.numeroTelefono,
                    'border-green-500': validFields.numeroTelefono,
                  }"
                  class="border rounded-md px-4 py-2 focus:outline-none mx-2"
                  @input="validateField(validFields.numeroTelefono)"
                />
                <span
                  v-show="!validFields.numeroTelefono"
                  class="ml-2 text-red-500"
                  >X</span
                >
                <span
                  v-show="validFields.numeroTelefono"
                  class="ml-2 text-green-500"
                  >✓</span
                >
              </div>

              <!-- Numero de celular -->
              <div class="mb-4 flex items-center text-start">
                <label class="block font-bold text-cyan-600 mr-2 text-sm"
                  >Número de celular</label
                >
                <input
                  type="text"
                  v-model="validFields.numeroCelular"
                  :class="{
                    'border-red-500': !validFields.numeroCelular,
                    'border-green-500': validFields.numeroCelular,
                  }"
                  class="border rounded-md px-4 py-2 focus:outline-none mx-2"
                  @input="validateField(validFields.numeroCelular)"
                />
                <span
                  v-show="!validFields.numeroCelular"
                  class="ml-2 text-red-500"
                  >X</span
                >
                <span
                  v-show="validFields.numeroCelular"
                  class="ml-2 text-green-500"
                  >✓</span
                >
              </div>
            </div>

            <div class="flex justify-between">
              <button
                type="button"
                class="px-4 py-2 bg-green-500 text-white rounded-lg mt-3"
                @click="prevStep"
              >
                Retroceder
              </button>
              <button
                type="button"
                class="px-4 py-2 bg-green-500 text-white rounded-lg mt-3"
                :class="{ 'btn-opaco': !isPaso2Valido }"
                :disabled="!isPaso2Valido"
                @click="nextStep"
              >
                Avanzar
              </button>
            </div>
          </div>

          <div v-show="currentStep === 3">
            <!-- Paso 3 -->
            <h3 class="text-xl font-bold mb-4">
              Paso 3: Información de residencia
            </h3>

            <div class="flex justify-between">
              <!-- Dirección de residencia -->
              <div class="mb-4 flex items-center text-start">
                <label class="block font-bold text-cyan-600 mr-2 text-sm"
                  >Dirección de residencia</label
                >
                <input
                  type="text"
                  v-model="validFields.direccionResidencia"
                  :class="{
                    'border-red-500': !validFields.direccionResidencia,
                    'border-green-500': validFields.direccionResidencia,
                  }"
                  class="border rounded-md px-4 py-2 focus:outline-none mx-2"
                  @input="validateField(validFields.direccionResidencia)"
                />
                <span
                  v-show="!validFields.direccionResidencia"
                  class="ml-2 text-red-500"
                  >X</span
                >
                <span
                  v-show="validFields.direccionResidencia"
                  class="ml-2 text-green-500"
                  >✓</span
                >
              </div>

              <!-- Código postal -->
              <div class="mb-4 flex items-center text-start">
                <label class="block font-bold text-cyan-600 mr-2 text-sm"
                  >Código postal</label
                >
                <input
                  type="text"
                  v-model="validFields.codigoPostal"
                  :class="{
                    'border-red-500': !validFields.codigoPostal,
                    'border-green-500': validFields.codigoPostal,
                  }"
                  class="border rounded-md px-4 py-2 focus:outline-none mx-2"
                  @input="validateField(validFields.codigoPostal)"
                />
                <span
                  v-show="!validFields.codigoPostal"
                  class="ml-2 text-red-500"
                  >X</span
                >
                <span
                  v-show="validFields.codigoPostal"
                  class="ml-2 text-green-500"
                  >✓</span
                >
              </div>
            </div>

            <div class="flex justify-between">
              <button
                type="button"
                class="px-4 py-2 bg-green-500 text-white rounded-lg mt-3"
                @click="prevStep"
              >
                Retroceder
              </button>

              <button
                type="button"
                class="px-4 py-2 bg-green-500 text-white rounded-lg mt-3"
                :class="{ 'btn-opaco': !isPaso3Valido }"
                :disabled="!isPaso3Valido"
                @click="enviarFormulario"
              >
                Enviar
              </button>
            </div>
          </div>

          <div
            v-show="formularioEnviado"
            class="fixed top-0 left-0 w-full h-full flex items-center justify-center bg-gray-800 bg-opacity-75"
          >
            <div class="bg-white rounded-lg p-6">
              <h4 class="text-2xl font-bold mb-4">Formulario enviado</h4>
              <p class="mb-4">¡El formulario ha sido enviado con éxito!</p>
              <button
                type="button"
                class="px-4 py-2 bg-green-500 text-white rounded-lg mt-3"
                @click="cerrarModal"
              >
                Cerrar
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import moment from "moment";

export default {
  data() {
    return {
      currentStep: 1,
      totalSteps: 3,

      validFields: {
        // Campos paso 1
        countries: [],
        selectedCountry: "",
        genero: "",
        primerNombre: "",
        segundoNombre: "",
        fechaNacimiento: "",
        tipoDocumento: "",
        numeroDocumento: "",
        fotoFrente: "",
        fotoReverso: "",
        // Campos paso 2
        correoElectronico: "",
        contrasena: "",
        confirmarContrasena: "",
        numeroTelefono: "",
        numeroCelular: "",
        // Campos paso 3
        direccionResidencia: "",
        codigoPostal: "",
      },

      formularioEnviado: false,
    };
  },
  mounted() {
    this.fetchCountries();
  },
  computed: {
    isPaso1Valido() {
      return (
        this.validFields.selectedCountry !== "" &&
        this.validFields.genero !== "" &&
        this.validFields.primerNombre !== "" &&
        this.validFields.fechaNacimiento !== "" &&
        this.validarEdad(this.validFields.fechaNacimiento) &&
        this.validFields.tipoDocumento !== "" &&
        this.validFields.numeroDocumento !== "" &&
        this.validFields.fotoFrente !== "" &&
        this.validFields.fotoReverso !== ""
      );
    },
    isPaso2Valido() {
      return (
        this.validarCorreoElectronico(this.validFields.correoElectronico) &&
        this.validFields.contrasena !== "" &&
        this.validFields.contrasena === this.validFields.confirmarContrasena &&
        this.validFields.numeroTelefono !== "" &&
        this.validFields.numeroCelular !== ""
      );
    },
    isPaso3Valido() {
      return (
        this.validFields.direccionResidencia !== "" &&
        this.validFields.codigoPostal !== ""
      );
    },
  },
  methods: {
    fetchCountries() {
      fetch("https://restcountries.com/v2/all")
        .then((response) => response.json())
        .then((data) => {
          this.validFields.countries = data;
        })
        .catch((error) => {
          console.error("Error:", error);
        });
    },
    handleCountryChange() {
      console.log(this.validFields.selectedCountry);
    },
    nextStep() {
      if (this.currentStep < this.totalSteps) {
        this.currentStep++;
      }
    },
    prevStep() {
      if (this.currentStep > 1) {
        this.currentStep--;
      }
    },
    validarEdad(fechaNacimiento) {
      const edadMinima = moment().subtract(18, "years");
      const fechaNacimientoMoment = moment(fechaNacimiento, "YYYY-MM-DD");
      return fechaNacimientoMoment.isSameOrBefore(edadMinima);
    },
    handleFrenteUpload(event) {
      const file = event.target.files[0];
      // Realizar acciones necesarias con el archivo cargado (guardar, validar, etc.)
      this.validFields.fotoFrente = file;
    },
    handleReversoUpload(event) {
      const file = event.target.files[0];
      // Realizar acciones necesarias con el archivo cargado (guardar, validar, etc.)
      this.validFields.fotoReverso = file;
    },
    validarCorreoElectronico(correo) {
      // Validar el formato de correo electrónico
      const regex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
      return regex.test(correo);
    },
    validateField(fieldName) {
      switch (fieldName) {
        case "pais":
          this.validFields.selectedCountry = this.selectedCountry !== "";
          break;
        case "genero":
          this.validFields.genero = this.genero !== "";
          break;
        case "primerNombre":
          this.validFields.primerNombre = this.primerNombre !== "";
          break;
        case "segundoNombre":
          this.validFields.segundoNombre = this.segundoNombre !== "";
          break;
        case "fechaNacimiento":
          this.validFields.fechaNacimiento = this.fechaNacimiento !== "";
          break;
        case "tipoDocumento":
          this.validFields.tipoDocumento = this.tipoDocumento !== "";
          break;
        case "numeroDocumento":
          this.validFields.numeroDocumento = this.numeroDocumento !== "";
          break;
        case "fotoFrente":
          this.validFields.fotoFrente = this.fotoFrente !== "";
          break;
        case "fotoReverso":
          this.validFields.fotoReverso = this.fotoReverso !== "";
          break;
        case "correoElectronico":
          this.validFields.correoElectronico = this.correoElectronico !== "";
          break;
        case "contrasena":
          this.validFields.contrasena = this.contrasena !== "";
          break;
        case "confirmarContrasena":
          this.validFields.confirmarContrasena =
            this.confirmarContrasena !== "";
          break;
        case "numeroTelefono":
          this.validFields.numeroTelefono = this.numeroTelefono !== "";
          break;
        case "numeroCelular":
          this.validFields.numeroCelular = this.numeroCelular !== "";
          break;
        case "direccionResidencia":
          this.validFields.direccionResidencia =
            this.direccionResidencia !== "";
          break;
        case "codigoPostal":
          this.validFields.codigoPostal = this.codigoPostal !== "";
          break;
      }
    },
    enviarFormulario() {
      // imprimir los valores en la consola
      console.log(
        "Dirección de residencia:",
        this.validFields.direccionResidencia
      );
      console.log("selectedCountry:", this.validFields.selectedCountry);
      console.log("genero:", this.validFields.genero);
      console.log("primerNombre:", this.validFields.primerNombre);
      console.log("segundoNombre:", this.validFields.segundoNombre);
      console.log("fechaNacimiento:", this.validFields.fechaNacimiento);
      console.log("tipoDocumento:", this.validFields.tipoDocumento);
      console.log("numeroDocumento:", this.validFields.numeroDocumento);
      console.log("fotoFrente:", this.validFields.fotoFrente);
      console.log("fotoReverso:", this.validFields.fotoReverso);
      // Campos paso 2
      console.log("correoElectronico:", this.validFields.correoElectronico);
      console.log("contrasena:", this.validFields.contrasena);
      console.log("confirmarContrasena:", this.validFields.confirmarContrasena);
      console.log("numeroTelefono:", this.validFields.numeroTelefono);
      console.log("numeroCelular:", this.validFields.numeroCelular);
      // Campos paso 3
      console.log("direccionResidencia:", this.validFields.direccionResidencia);
      console.log("codigoPostal:", this.validFields.codigoPostal);

      // Mostrar el modal de confirmación
      this.formularioEnviado = true;
    },
    cerrarModal() {
      // Cerrar el modal de confirmación y reiniciar el formulario
      this.formularioEnviado = false;
      this.currentStep = 1;

      // Campos paso 1
      (this.validFields.selectedCountry = ""),
        (this.validFields.genero = ""),
        (this.validFields.primerNombre = ""),
        (this.validFields.segundoNombre = ""),
        (this.validFields.fechaNacimiento = ""),
        (this.validFields.tipoDocumento = ""),
        (this.validFields.numeroDocumento = ""),
        (this.validFields.fotoFrente = ""),
        (this.validFields.fotoReverso = ""),
        // Campos paso 2
        (this.validFields.correoElectronico = ""),
        (this.validFields.contrasena = ""),
        (this.validFields.confirmarContrasena = ""),
        (this.validFields.numeroTelefono = ""),
        (this.validFields.numeroCelular = ""),
        // Campos paso 3
        (this.validFields.direccionResidencia = ""),
        (this.validFields.codigoPostal = "");
    },
  },
};
</script>

<style>
/* Estilos personalizados */
body {
  font-family: "Roboto", sans-serif;
}

.container {
  padding-top: 2rem;
}

input[type="text"],
input[type="number"],
input[type="date"],
input[type="email"],
input[type="file"],
input[type="password"] {
  padding: 0.5rem;
  /* border: 1px solid #ccc; */
  border-radius: 0.55rem;
  width: 260px;
}

select {
  width: 260px;
}

button {
  cursor: pointer;
}

/* Estilos para el modal de confirmación */
.modal {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: rgba(0, 0, 0, 0.75);
  z-index: 9999;
}

.modal-content {
  background-color: #fff;
  padding: 2rem;
  border-radius: 0.5rem;
  text-align: center;
}

.modal-button {
  margin-top: 1rem;
  padding: 0.5rem 1rem;
  background-color: #48bb78;
  color: #fff;
  border-radius: 0.25rem;
  cursor: pointer;
}

.btn-opaco {
  opacity: 0.5;
  background-color: black !important;
}

label {
  /* border: solid; */
  width: 90px;
  min-width: 90px;
  text-align: left;
}

span {
  margin-left: 35px;
  margin-right: 35px;
}
</style>
