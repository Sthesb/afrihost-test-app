<template>
  <div class="home">
    <button @click="toggleFilters">Show Filters</button>

    <table class="styled-table">
        <thead>
            <tr>
                <th>Name and ID</th>
                <th>Next Service</th> 
                <th></th> 
            </tr>
        </thead>
        <tbody>
            <tr v-for="vehicle in vehicles_data" :key="vehicle.id">
                <td >
                  <div class="avatar">
                    <avatar :size="60" :fullname="vehicle.name" color="#cad7f9"></avatar>
                    <div>
                      <h4>{{ vehicle.name }}</h4>
                      <p>#{{ vehicle.trips }}</p>
                    </div>
                  </div>
                  
                </td>
                <td>{{ vehicle.service_due }}</td>
                <td><router-link class="link" :to="{name: ''}" >Manage</router-link></td>
            </tr>
            <!-- and so on... -->
        </tbody>
    </table>

    <transition name="filters">
      <div class="filters" v-show="showFilters">
        <i class="fa fa-times close" @click="toggleFilters" aria-hidden="true"></i>
        <h3>Filters</h3>
        <div class="slidecontainer">
          <label for="">Trips taken: {{ trips }}</label>
          <!-- <input type="range"  min="0 " max="100" class="slider" v-model="trips"> -->
          <vue-slider v-model="trips" v-bind="tripOptions" class="slider"></vue-slider>
        </div>
        <div class="slidecontainer">
          <label for="">Service due: {{ serviceDate }}</label>
          <!-- <input type="range"  min="1" max="31" class="slider" v-model="service"> -->
          <vue-slider v-model="service" v-bind="serviceOptions" @change="handleService" class="slider"></vue-slider>
        </div>
        <div class="slidecontainer">
          <label for="">Vehicle model: {{ model }}</label>
          <input type="text" placeholder="Vehicle model"  class="input" v-model="model">
        </div>
        <div class="slidecontainer">
          <label for="">Status: {{ status }}</label>
          <input type="text" placeholder="Status"  class="input" v-model="status">
        </div>
        <div class="slidecontainer">
          <label for="">Location: {{ location }}</label>
          <input type="text" placeholder="Location"   class="input" v-model="location">
        </div>
        <div class="slidecontainer">
          <button @click="filter">Filter</button>
        </div>
      </div>
    </transition>
  </div>

  
</template>

<script>
// @ is an alias to /src
import Avatar from 'vue-avatar-component'
import VueSlider from 'vue-slider-component'
import 'vue-slider-component/theme/default.css'

export default {
  name: 'HomeView',
  components: {
    Avatar,
    VueSlider
  },
  data(){ 
    return {
      data : {},
      vehicles_data : {},
      api_url: 'https://www.afrihost.com/resources/fedev/mid/',
      showFilters: false,
      trips: null,
      service: null,
      serviceDate: null,
      model: '',
      status: '',
      location: '',
      tripOptions: {
        min: null,
        max: 0,
        height: 10,
        dataValue: 0
      },
      serviceOptions: {
        tooltip: { isVisible: false },
        min: new Date("2022-04-14").getTime(),
        max: new Date("2024-10-10").getTime(),
        height: 10,
        stepVal:86400000,
        // Slider ticks customization
        ticks: { placement: 'After', largeStep:  2 * 86400000 }
      },

    }
  },
  created( ) {
    fetch(this.api_url, {
      headers: new Headers({
        'Accept': '*'
      })
    })
    .then(response => response.json())
    .then(res => {
      console.log(res)
      let trips = [];
       Array(res['vehicles'].forEach(element => {
        // console.log(element.trips)
        return trips.push(element.trips)
      }));

      // console.log(trips)
      // this.tripOptions.min = Math.min(...trips)
      this.tripOptions.max = Math.max(...trips)
      this.trips = Math.max(...trips)
      this.data = res['vehicles']
      return this.vehicles_data = res['vehicles']
    })
    .catch(err => console.log(err.message))
    

  },
  
  methods: {
    toggleFilters() {
      this.showFilters = !this.showFilters
    },
    changeFoo() {
        this.$emit('changeFoo', this.trips);
    },
    handleService(){
      console.log(new Date(this.service))
      let year = new Date(this.service).getFullYear()
      let month = new Date(this.service).getMonth()
      let day = new Date(this.service).getDay()

      
      
      this.serviceDate = year + '-' + month + '-' + day;
    },
    filter(){
      this.vehicles_data = this.data.filter((vehicle) => {
        return vehicle.trips <= this.trips && vehicle.model.includes(this.model) ;
      })

      
    }
  },
  
}
</script>


<style lang="scss" scoped> 
  .home{
    padding-top: 50px;
    margin: 0px;
    // border: 1px solid gray;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;

    button{
      width: 50%;
      padding: 10px 0;
      background-color: blue;
      color: white;
      border: 1px solid transparent;
      border-radius: 5px;
      font-size: 16px;
      font-weight: bold;
      cursor: pointer;
    }

   

    .styled-table {
        border-collapse: collapse;
        margin: 25px 0;
        font-size: 0.9em;
        font-family: sans-serif;
        width: 100%;
        // box-shadow: 0 0 20px rgba(0, 0, 0, 0.15);

        thead tr {
            background-color: transparent;
            color: #353637;
            text-align: left;
            
        }

        th, td {
            padding: 12px 15px;
        }
        
        tbody, tr {
          background-color: white;
          border-bottom: 1px solid #dddddd;
          padding: 200px 0 !important;
          margin: 200px 0 !important;
          bottom: 5px;
        }
        // tbody tr:nth-of-type(even) {
        //     background-color: #f3f3f3;
        // }

        // tbody tr:last-of-type {
        //     border-bottom: 2px solid #009879;
        // }

        // tbody tr.active-row {
        //     font-weight: bold;
        //     color: #009879;
        // }

        .avatar {
          display: flex;
          align-items: center;
          div {
            margin-left: 5px;
            h4 {
              font-size: 14px;
              font-weight: bold;
              text-transform: capitalize;
            }
          }
        }

        .link {
          text-decoration: none;
          font-size: 14px;
          font-weight: bold;
          color: blue;
        }
    }

    .filters {
        
        padding-left: 20px;
        display: flex;
        flex-direction: column;
        position: fixed;
        width: 100%;
        z-index: 1;
        max-width: 300px;
        height: 100%;
        background-color: white;
        top: 0;
        left: 0;

        .close {
            padding-top: 15px ;
            margin-bottom: 80px;
            padding-right: 15px ;
            top: 0;
            text-align: right;
            right: 0;
            color: red;
            font-size: 20px;
        }
        
        h3{
          margin-bottom: 20px;
          font-size: 20px;
          font-weight: 300;
          text-transform: uppercase;
          color: gray;
        }

        @media (max-width: 750px) {
            max-width: 250px;
        }

        .slidecontainer {
          width: 90%;
          display: flex;
          flex-direction: column;
          margin-bottom: 30px;

          label {
            margin-bottom: 5px;
          }

          .input {
            padding: 10px 0;
            padding-left: 5px;
            border-radius: 5px;
            border-color: rgb(164, 164, 164);

            &:active {
              border-color: blue;
            }

            
          }
          
        }

        

    }


  }
  
</style>