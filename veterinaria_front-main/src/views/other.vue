<template>
  <div class="screen">
    <h1><strong>Administarador de pacientes de veterinaria </strong></h1>
    <div class="position">
      <div class="position_left">
        <h2><strong>DATOS DE LA MASCOTA:</strong></h2>
        <p></p>
        <label><strong>Nombre de mascota</strong></label>
        <input class="position_left_input" v-model="Nombre_Mascota" type="text">
        <label><strong>Propietario</strong></label>
        <input class="position_left_input" v-model="Propietario" type="text">
        <label><strong>Telefono</strong></label>
        <input class="position_left_input" v-model="Telefono" type="text">
        <label><strong>Fecha</strong></label>
        <input class="position_left_input" v-model="Fecha" type="date">
        <label><strong>Hora</strong></label>
        <input class="position_left_input" v-model="Hora" type="time">
        <label><strong>Sintomas</strong></label>
        <textarea class="position_left_textarea" v-model="Sintomas" cols="30" rows="5"></textarea>
        <input class="position_button" @click="Enviar" type="button" value="Enviar">
        </div>
        <div class="position_right">
          <h2><strong>Mostrar las Citas</strong></h2>
          <v-data-table
            :headers="encabezados"
            :items="cita"
          ></v-data-table>

      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {

  data(){
    return{
      
      bd:0,
      cita:[],
      encabezados: [
          {
            text: 'mascota',
            align: 'start',
            sortable: false,
            value: 'Nombre_Mascota',
          },
         
          { text: 'dueño', value: 'Propietario' },
          { text: 'telefono', value: 'Telefono' },
          { text: 'hora', value: 'Hora' },
          { text: 'fecha', value: 'Fecha' },
          { text: 'sintomas', value: 'Sintomas' },
        ],
    };
  },

  created(){
    this.recibir()
  },

    methods: {
      recibir(){
          axios.get(`cita`)
          .then((response) =>{
            this.cita = response.data.formulario;
            console.log(this.cita);
          })
          .catch(error => {
            console.log(error.response)
          });
      },

     Enviar(){
      if (this.bd == 0){
        axios.post(
          `cita`,
          {
          Nombre_Mascota:this.Nombre_Mascota,
          Propietario:this.Propietario,
          Telefono:this.Telefono,
          Hora:this.Hora,
          Fecha:this.Fecha,
          Sintomas:this.Sintomas
          },
        )        
        .then((response) => {
        console.log(response);
        this.recibir()
        this.vaciar()
        })
        .catch((error) => {
          console.log (error.response);
        })
        } 
      },

      vaciar(){
        this.Nombre_Mascota = '', 
        this.Propietario = '',
        this.Telefono = '',
        this.Hora = '',
        this.Fecha = '',
        this.Sintomas = ''
      }
  },    
}

</script>

<style scoped>
  
.screen{
  background: linear-gradient(to bottom, rgb(132, 39, 238), rgb(125, 192, 255));
  height:100%;
  margin:0;
}

.screen h1{
  text-align: center;
  padding-top: 50px;
  font-size: 40px;
  color: white;
}

.position{
  padding-top: 60px;
  display: flex;
  justify-content: space-around;
}

.position div{
  background-color: rgba(255, 255, 255, 0.459);
  border-radius: 20px;
  padding: 20px;
}

.position_left{
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 45%;
  max-width: 700px;
  min-width: 400px;
}

.position_left label{
  text-align: left;
  padding-left: 40px;
  padding-top: 20px;
  font-size: 20px;
  color: rgb(75, 75, 75);
}

.position_left h2{
  padding-top: 20px;
}

.position_left_input{
  background-color: #fff;
  color: rgb(82, 82, 82);
  height: 20px;
  min-width: 150px;
  width: 90%;
  max-width: 440px;
  height: 30px;
  border: rgba(255, 255, 255, 0) 1px solid;
  border-bottom: #ccc 2px solid;
  padding: 8px;
  margin-top:10px;
  font-size:1em;
  border-radius:4px;
}

.position_left_textarea{
  background-color: #fff;
  color: rgb(82, 82, 82);
  height: 100px;
  min-width: 150px;
  width: 80%;
  max-width: 440px;
  border: rgba(255, 255, 255, 0) 1px solid;
  border-bottom: #ccc 2px solid;
  padding: 8px;
  margin-top:10px;
  font-size:1em;
  border-radius:4px;
}

.position_button{
  height: 40px;
  width: 200px;
  background:#2ecc71;
  color:white;
  border-radius:4px;
  border-style: none;
  border-radius: 25px;
  margin-top: 20px;
  margin-bottom: 20px;
}

/* Right */

.position_right{
  display: flex;
  flex-direction: column;
  align-items: center;

  width: 45%;
  max-width: 800px;
  min-width: 400px;
}

.position_right h2{
  padding-top: 20px;
}

</style>