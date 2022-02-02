<template>
  <div class="container">
    <header class="header">
      <h1>UITEC App</h1>
    </header>
    <main>
      <div class="tabla1">
        <h2>Equipos</h2>
        <table>
          <tr>
            <th>Nombres</th>
          </tr>
          <tr>
            <td>Computadora</td>
          </tr>
          <tr>
            <td>Arduino</td>
          </tr>
          <tr>
            <td>Router</td>
          </tr>
        </table>
      </div>
      <div class="tabla2">
        <h2>Estudiantes</h2>
        <table>
          <tr>
            <th>Nombre</th>
            <th>Equipo</th>
            <th>Estado</th>
            <th>Fecha Adquisición</th>
            <th>Fecha Máxima</th>
            <th>Fecha Devuelto</th>
          </tr>
          <tr v-for="usuario in usuarios">
            <td>{{ usuario.nombre }}</td>
            <td>{{ usuario.equipo }}</td>
            <td v-bind:class="usuario.estado">{{ usuario.estado }}</td>
            <td>{{ usuario.fechaAdquisicion }}</td>
            <td>{{ usuario.fechaMaxima }}</td>
            <td>{{ usuario.fechaDevuelto }}</td>
          </tr>
        </table>
      </div>
      <div class="tabla3">
        <h2>Pagos Pendientes</h2>
        <table>
          <tr>
            <th>Estudiante</th>
            <th>Equipo</th>
            <th>Total a Pagar</th>
          </tr>
          <tr v-for="(usuario, index) in usuarios">
            <td>{{ usuario.nombre }}</td>
            <td>{{ usuario.equipo }}</td>
            <td>$ {{ usuarios[index].deuda }}</td>
          </tr>
        </table>
      </div>
    </main>
    <footer>
      <p>&#169 Creado por <a href="">Hernan Berrazueta</a> 2022</p>
    </footer>
  </div>
</template>

<script>
export default {
  name: "Home",
  data() {
    return {
      usuarios: [
        {
          nombre: "Hernán Berrazueta",
          equipo: "Arduino",
          estado: "Entregado",
          fechaAdquisicion: "01/01/2022",
          fechaMaxima: "01/15/2022",
          fechaDevuelto: "01/17/2022",
        },
        {
          nombre: "Nicolás Benavides",
          equipo: "Arduino",
          estado: "Pendiente",
          fechaAdquisicion: "01/12/2022",
          fechaMaxima: "01/20/2022",
          fechaDevuelto: "",
        },
        {
          nombre: "Sebastián Aliaga",
          equipo: "Computadora",
          estado: "Pendiente",
          fechaAdquisicion: "01/24/2022",
          fechaMaxima: "01/27/2022",
          fechaDevuelto: "",
        },
        {
          nombre: "Fernando Orbe",
          equipo: "Router",
          estado: "Entregado",
          fechaAdquisicion: "01/20/2022",
          fechaMaxima: "01/23/2022",
          fechaDevuelto: "01/24/2022",
        },
        {
          nombre: "Luis Sánchez",
          equipo: "Computadora",
          estado: "Entregado",
          fechaAdquisicion: "02/10/2021",
          fechaMaxima: "01/11/2022",
          fechaDevuelto: "",
        },
      ],
    };
  },
  methods: {
    CalcularDeuda(fechaDeEntrega, fechaDevuelto) {
      const today = new Date().toLocaleDateString("en-US", {
        timeZone: "America/Guayaquil",
      });
      const today2 = new Date(today);
      let tiempoMilisegundos;
      if (!isNaN(fechaDevuelto)) {
        tiempoMilisegundos = Math.abs(fechaDevuelto - fechaDeEntrega);
      } else {
        tiempoMilisegundos = Math.abs(today2 - fechaDeEntrega);
      }
      const diasDeAtraso = Math.ceil(
        tiempoMilisegundos / (1000 * 60 * 60 * 24)
      );
      //console.log(`Usted tiene ${diasDeAtraso} días de retraso, por lo tanto debe $${diasDeAtraso*5} dolares`)
      return diasDeAtraso * 5;
    },
  },
  created() {
    this.usuarios.forEach((element) => {
      const fecha = new Date(element.fechaMaxima);
      const fechaDevuelto = new Date(element.fechaDevuelto);
      element.deuda = this.CalcularDeuda(fecha, fechaDevuelto);
    });
  },
};
</script>

<style scoped>

.Entregado{
    background-color:green;
    color: #FFF;
}

.Pendiente{
    background-color:red;
    color: #FFF;
}

h1 {
  text-align: center;
  font-size: 240%;
  color: #1590b8;
}

h2 {
  text-align: center;
}

table {
  font-family: arial, sans-serif;
  border-collapse: collapse;
  max-width: 100%;
  margin: auto;
}

td{
    padding: 10px 15px !important;
}

td,
th {
  border: 1px solid #dddddd;
  text-align: left;
  padding: 8px;
}

th {
  text-align: center;
}

.tabla1 {
  margin-top: 5%;
}

.tabla2 {
  margin-top: 6%;
}

.tabla3 {
  margin-top: 6%;
}

footer {
  margin-top: 10%;
  text-align: center;
}

a {
  color: royalblue;
}
</style>
