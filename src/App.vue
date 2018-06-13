<template>
  <div id="app">
    <img src="./assets/logo.png">
    <h1>{{ msg }}</h1>

    <form>

        <div class="form-group">
            <select class="form-control" name="city" v-model="cityModel">
                <option value="" disabled selected>Select City</option>
                <option v-for="city in cities" :value="city.id" :key="city.id">{{city.name}}</option>
            </select>
        </div>

        <div class="form-group">
            <input type="text" name="date" class="form-control" v-model="date"  placeholder="14-06-2018">
        </div>

        <div v-if="respon">
            <div class="respon" v-for="index in respon.max.length">
                {{respon.name}}  <br>
                Max Temperature : {{respon.max[index-1]}} <br>
                Min Temperature : {{respon.min[index-1]}} <br>

                <img :src="getImgUrl(index-1)" >
                <hr>
            </div>
        </div>

        <button @click.prevent='getTemprature(cityModel)'>Get Temprature</button>
    </form>
    

    
  </div>
</template>

<script>
export default {
    name: 'app',
    data () {
        return {
            msg: 'Welcome to the small workshop',
            cities: [],
            cityModel: '',
            date: '',
            respon: ''
        }
    }, 
    methods: {
        getImgUrl(index){
            return 'http://openweathermap.org/img/w/' + this.respon.icon[index]
        },
        callApi() {
            this.$http.get('http://localhost:8000/api/cities')
                .then(function(respon){
                    for (var i = respon.body.length - 1; i >= 0; i--) {
                        this.cities.push(respon.body[i])
                    }
                })
        },
        getTemprature() {
            this.$http.get('http://localhost:8000/api/getTemprature/'+ this.cityModel + '/'+ this.date)
                .then(function(respon){
                    this.respon = JSON.parse(respon.bodyText);
                })
        }

    },
    beforeMount(){
        this.callApi()
    }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

h1, h2 {
  font-weight: normal;
}

.form-control {
    width: 100%;
    margin-bottom: 10px;
    padding: 0.5rem 0.75rem;
    font-size: 1rem;
    line-height: 1.25;
    color: #464a4c;
    background-color: #fff;
    background-image: none;
    -webkit-background-clip: padding-box;
    background-clip: padding-box;
    border: 1px solid rgba(0, 0, 0, 0.15);
    border-radius: 0.25rem;
}

.form-group {
    width: 30%;
    margin: auto;
}

select { width: 22% }

</style>
