<template>
  <f7-page>
    <f7-navbar v-if="$theme.material">
      <f7-nav-left>
        <f7-link icon="icon-bars" open-panel="left"></f7-link>
      </f7-nav-left>
      <f7-nav-center sliding>{{$lang('carte')}}</f7-nav-center>
    </f7-navbar>
    <gmap-map ref="map"
      :center="{lat:center.lat, lng:center.lng}"
      :options="{disableDefaultUI:false}"
      :zoom="15"
      style="width: 360px; height: 380px">
      <gmap-marker
        :key="index"
        v-for="(m, index) in markers"
        :position="m.position"
        :clickable="true"
        :draggable="true"
        @click="center=m.position"
      ></gmap-marker>
    </gmap-map>
    <f7-block>
      <form class="list-block">
            <div class="row">
               <div class="item-content col-80">
                  <div class="item-inner">
                    <div class="item-title label">{{$lang('recherche')}}</div>
                    <div class="search item-input">
                      <input type="text" v-model="searchAddressInput" id="searchmap" v-on:change="searchLocation()"></input>
                    </div>
                  </div>
                </div>
              <a href="#">
                <div class="geolocation col-20" v-on:click="geolocation()" align="right">
                  <img src="map-marker.png" width="50"/>
                </div>
              </a>
              <div class="item-content col-80">
                <div class="item-inner">
                  <div class="item-title label">{{$lang('modedeplacement')}}</div>
                  <select class="item-select" id="mode" v-on:change="calculate()">
                    <option value="DRIVING">{{$lang('conduite')}}</option>
                    <option value="WALKING">{{$lang('marche')}}</option>
                    <option value="BICYCLING">{{$lang('bicyclette')}}</option>
                    <option value="TRANSIT">{{$lang('transit')}}</option>
                  </select>
                </div>
              </div>
              <a href="#">
                <div class="calculate col-20" v-on:click="calculate()" align="right">
                  <img src="map-trajet.jpg" width="50"/>
                </div>
              </a>
            </div>
          </div>
      </form>

    </f7-block>
  </f7-page>
</template>
<script>
export default {
    data () {
      return {
        currentLocation : {lat: 10.0, lng: 10.0},
        searchAddressInput: '',
        center: {lat: 10.0, lng: 10.0},
        destination: {lat: 10.0, lng: 10.0},
        markers: [{
          position: {lat: 10.0, lng: 10.0},
        }, {
          position: {lat: 11.0, lng: 11.0}
        }]
      }
    },

    mounted : function() {
      this.geolocation();
    },
    methods: {
    geolocation : function() {
      navigator.geolocation.getCurrentPosition((position) => {
        this.currentLocation = {
          lat: position.coords.latitude,
          lng: position.coords.longitude
        };
        this.center = {
          lat: position.coords.latitude,
          lng: position.coords.longitude
        };
        this.markers[0].position = {
          lat: position.coords.latitude,
          lng: position.coords.longitude
        }
        this.$fireDB('currentLocation').set({lat: position.coords.latitude, lng: position.coords.longitude});
      });
    },
    searchLocation: function() {
      var geocoder = new google.maps.Geocoder();
      geocoder.geocode({'address': this.searchAddressInput}, (results, status) => {
        if (status === 'OK') {
          this.center = {
            lat : results[0].geometry.location.lat(),
            lng : results[0].geometry.location.lng()
          }
          this.destination = {
            lat : results[0].geometry.location.lat(),
            lng : results[0].geometry.location.lng()
          }
          this.markers[1].position = {
            lat: this.destination.lat,
            lng: this.destination.lng
          }
          this.$fireDB('center').set({lat: results[0].geometry.location.lat(), lng: geometry.location.lng()});
        }
      })
    },
    calculate: function(){
    var origin      = new google.maps.LatLng(this.currentLocation.lat, this.currentLocation.lng); // Le point départ
    var destination = new google.maps.LatLng(this.destination.lat, this.destination.lng); // Le point d'arrivé
    var selectedMode = document.getElementById('mode').value;
    if(origin && destination){
        var request = {
            origin      : origin,
            destination : destination,
            travelMode  : google.maps.TravelMode[selectedMode] // Mode de conduite
        }
        var directionsService = new google.maps.DirectionsService(); // Service de calcul d'itinéraire
        var directionsDisplay = new google.maps.DirectionsRenderer(); // Service de Tracé de l'itinéraire
            directionsDisplay.setMap(this.$refs.map.$mapObject);
            directionsService.route(request, function(response, status){ // Envoie de la requête pour calculer le parcours
            if(status == google.maps.DirectionsStatus.OK){
                directionsDisplay.setDirections(response); // Trace l'itinéraire sur la carte et les différentes étapes du parcours
            }
        })
      }
    },
      created() {
        this.$fireDB('currentLocation').on('value', (snap) => {
          this.$db('currentLocation', snap.val())
          this.$db('center', snap.val())
      })
    }
  },
}
function currentLocation() {
  var currentLocation = this.$db('currentLocation');
  return JSON.parse(JSON.strignify(currentLocation));
}
</script>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           
