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
                    <span class="msg"> @{{ action_message }} </span>
                </form>
            </div>
        </section>
        <section class="ajax-section">
            <div class="container">
                <ul class="cities">
                    <li class="city">
                        <h2 class="city-name" data-name="Japan,JP">
                            <span>Japan</span>
                            <sup>JP</sup>
                        </h2>
                        <div class="city-temp">4<sup>°C</sup></div>
                        <figure>
                            <img class="city-icon" src="https://s3-us-west-2.amazonaws.com/s.cdpn.io/162656/02n.svg" alt="few clouds">
                            <figcaption>few clouds</figcaption>
                        </figure>
                    </li>
                </ul>
            </div>
        </section>
    </div>
</template>

<script>
import axios from 'axios';
export default {
    data() {
        return {
            test: 'test',
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
            error: null,
            action_message: ""
        }
    },

    methods: {
        retrieveWeather: function() {
            var self = this;
            self.error = '';

            axios.get('https://cors-anywhere.herokuapp.com/http://api.openweathermap.org/data/2.5/weather?q=Osaka,japan&APPID=5586df44a060b76dd847e3ffbfdee78e')
                .then(function(data) {

                    console.log(data)
                }).catch(function(error) {
                    self.error = "Something went wrong. Please try again."
                })
        },
        onSubmit: function() {
            if (this.selected_city == "") {
                alert('Kindly select a city first')
            } else {
               this.action_message="Please wait while we look up through your request";
            }

        }
    },
    mounted() {

        this.retrieveWeather();
    },
}
</script>
