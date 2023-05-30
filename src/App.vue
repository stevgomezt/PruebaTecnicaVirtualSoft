<template>
  <div id="app">
    <FormularioVirtualsoft class="fondo" />
  </div>
</template>

<script>
import FormularioVirtualsoft from "./components/FormularioVirtualsoft.vue";

export default {
  name: "App",
  components: {
    FormularioVirtualsoft,
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  /* margin-top: 60px; */
}
.fondo {
  background-image: url("data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' version='1.1' xmlns:xlink='http://www.w3.org/1999/xlink' xmlns:svgjs='http://svgjs.com/svgjs' width='1920' height='1080' preserveAspectRatio='none' viewBox='0 0 1280  1024'%3e%3cg mask='url(%26quot%3b%23SvgjsMask1147%26quot%3b)' fill='none'%3e%3crect width='1920' height='1080' x='0' y='0' fill='rgba(21%2c 67%2c 96%2c 1)'%3e%3c/rect%3e%3cpath d='M0%2c483.749C119%2c482.735%2c236.096%2c584.366%2c345.588%2c537.746C455.658%2c490.88%2c518.327%2c366.193%2c548.178%2c250.345C575.837%2c143.002%2c516.391%2c37.722%2c502.777%2c-72.288C488.805%2c-185.188%2c546.178%2c-322.962%2c467.108%2c-404.751C387.33%2c-487.272%2c245.468%2c-429.505%2c133.412%2c-454.358C31.637%2c-476.931%2c-55.749%2c-552.101%2c-159.663%2c-543.764C-274.376%2c-534.561%2c-389.708%2c-489.828%2c-468.617%2c-406.059C-548.815%2c-320.921%2c-599.14%2c-201.806%2c-591.734%2c-85.078C-584.742%2c25.128%2c-470.097%2c93.51%2c-430.382%2c196.549C-386.768%2c309.704%2c-450.265%2c476.352%2c-349.393%2c543.665C-249.072%2c610.611%2c-120.603%2c484.776%2c0%2c483.749' fill='%230e2c3e'%3e%3c/path%3e%3cpath d='M1920 1714.042C2032.9470000000001 1684.891 2109.555 1590.1309999999999 2212.046 1534.432 2325.522 1472.763 2497.799 1485.82 2553.956 1369.518 2609.886 1253.687 2512.23 1122.342 2469.629 1000.975 2434.816 901.795 2395.117 807.3530000000001 2328.182 726.308 2259.943 643.684 2175.696 581.424 2081.274 530.751 1966.211 469 1850.503 389.28599999999994 1720.367 400.11300000000006 1580.821 411.72299999999996 1443.196 481.711 1355.792 591.111 1269.5439999999999 699.063 1266.0720000000001 846.582 1253.714 984.203 1241.946 1115.25 1242.077 1245.524 1285.884 1369.591 1332.652 1502.045 1388.373 1648.113 1511.806 1715.162 1633.842 1781.452 1785.529 1748.749 1920 1714.042' fill='%231c5a82'%3e%3c/path%3e%3c/g%3e%3cdefs%3e%3cmask id='SvgjsMask1147'%3e%3crect width='1920' height='1080' fill='white'%3e%3c/rect%3e%3c/mask%3e%3c/defs%3e%3c/svg%3e");
}
</style>
