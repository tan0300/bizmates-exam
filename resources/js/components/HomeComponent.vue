<template>
    <div id="wrapper">
        <section class="top-banner">
            <div class="container">
                <h1 class="heading">Welcome to Japan!</h1>
                <form method="GET" action="#" @submit.prevent="onSubmit">
    
                    <div class="select">
                        <select v-model="selected_city" required>
                                                               <option value="">Please select a city</option>
                                                                <option v-for="city in available_cities" :key="city" :value="city">{{ city }}</option>
                                                            </select>
                    </div>
                    <button type="submit">SUBMIT</button>
                    <span class="msg"> {{ action_message }} </span>
                </form>
            </div>
        </section>
        <section class="ajax-section" v-if="weather.length != 0">
            <div class="container">
                <div class="row">
                    <div id="weather" class="col">
                        <h2 class="city-name">
                            <span>{{ selected_city }}</span>
    
                        </h2>
                        <div class="city-temp">{{ farenheightToCelsius }}<sup>°C</sup></div>
                        <figure>
                            <img class="city-icon" src="../../images/cloud.svg" alt="few clouds">
                            <figcaption>{{ weather.weather[0].description }}</figcaption>
                        </figure>
                    </div>
    
                    <div class="col">
                        <h1>
                            <span>Places you might want to visit in the city</span></h1>
                        <div class="places">
                            <ol>
                                <li v-for="places in recommended_places" :key="places.referralId"> {{ places.venue.name}}
                                    <p> {{ places.reasons.items[0].summary }} </p>
                                </li>
                            </ol>
                        </div>
                        <p>{{ action_message }}</p>
                    </div>
    
                </div>
            </div>
        </section>
    </div>
</template>

<script>
import axios from 'axios';
export default {
    data() {
        return {

            available_cities: [
                'Tokyo',
                'Yokohama',
                'Kyoto',
                'Osaka',
                'Sapporo',
                'Nagoya'
            ],
            selected_city: "",
            weather: null,
            action_message: "",
            weather: [],
            recommended_places: [],

        }
    },

    methods: {
        retrieveWeather: function() {
            var self = this;
            self.error = '';

            axios.get('https://cors-anywhere.herokuapp.com/http://api.openweathermap.org/data/2.5/weather?q=' + self.selected_city + ',japan&APPID=5586df44a060b76dd847e3ffbfdee78e')
                .then(function(data) {

                    console.log(data)
                    self.action_message = "";
                    self.weather = data.data;
                }).catch(function(error) {
                    self.error = "Something went wrong. Please try again."
                })
        },

        retreiveRecommendedPlaces: function() {
            var self = this;
            axios.get('https://cors-anywhere.herokuapp.com/https://api.foursquare.com/v2/venues/explore?client_id=W4DLCG1PEXBLFNKHM4LY5FFGVEYDYVYQ3BHQTTB04Z5ZL5TN&client_secret=GLCTNS1O4AQZE1L344JU5VJXQ025WFXTHP4FYUQD4XPGXSHT&v=20180323&categoryId=4deefb944765f83613cdba6e&near=Osaka&limit=5')
                .then(function(data) {

                    console.log(data, "places")
                    self.action_message = "";
                    self.recommended_places = data.data.response.groups[0].items;
                }).catch(function(error) {
                    self.error = "Something went wrong. Please try again."
                })
        },
        onSubmit: function() {
            if (this.selected_city == "") {
                alert('Kindly select a city first')
            } else {
                this.action_message = "Please wait while we look up through your request";
                this.retrieveWeather();
                this.retreiveRecommendedPlaces();
            }

        }
    },
    mounted() {


    },

    computed: {
        farenheightToCelsius: function() {
            if (this.weather.length != 0) {
                return (this.weather.main.feels_like - 273.15).toFixed(1);
            }
        }
    }
}
</script>
