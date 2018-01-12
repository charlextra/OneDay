<template>
  <f7-page>
    <f7-navbar v-if="$theme.material">
      <f7-nav-left>
        <f7-link icon="icon-bars" open-panel="left"></f7-link>
      </f7-nav-left>
      <f7-nav-center sliding>{{$lang('carte')}}</f7-nav-center>
    </f7-navbar>
    <gmap-map
      :center="{lat:currentLocation.lat, lng:currentLocation.lng}"
      :options="{disableDefaultUI:false}"
      :zoom="15"
      style="width: 320px; height: 380px">
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
      <form class="list-block inputs-list">
            <div class="row">
              <div class="item-content col-80">
                <div class="item-inner">
                    <div class="item-title label">{{$lang('recherche')}}</div>
                    <div class="search item-input">
                      <input type="text" v-model="searchAddressInput" v-on:change="searchLocation()"></input>
                    </div>
                  </div>
                </div>            <div>{{$db('currentLocation')}}</div>
            <a href="#">
              <div class="geolocation col-20" v-on:click="geolocation()" align="right">
                <img src="map-marker.png" width="50"/>
              </div>
            </a>
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
        this.$fireDB('currentLocation').set({lat: position.coords.latitude, lng: position.coords.longitude});
      });
    },
    searchLocation: function() {
      var geocoder = new google.maps.Geocoder();
      geocoder.geocode({'address': this.searchAddressInput}, (results, status) => {
        if (status === 'OK') {
          this.currentLocation.lat = results[0].geometry.location.lat();
          this.currentLocation.lng = results[0].geometry.location.lng();
        }
      })
    },
    calculate: function(){
    origin      = document.getElementById('origin').value; // Le point départ
    destination = document.getElementById('destination').value; // Le point d'arrivé
    if(origin && destination){
        var request = {
            origin      : origin,
            destination : destination,
            travelMode  : google.maps.DirectionsTravelMode.DRIVING // Mode de conduite
        }
        var directionsService = new google.maps.DirectionsService(); // Service de calcul d'itinéraire
        directionsService.route(request, function(response, status){ // Envoie de la requête pour calculer le parcours
            if(status == google.maps.DirectionsStatus.OK){
                direction.setDirections(response); // Trace l'itinéraire sur la carte et les différentes étapes du parcours
            }
        })
      }
    },
      created() {
        this.$fireDB('currentLocation').on('value', (snap) => {
        this.$db('currentLocation', snap.val())
      })
    }
  },
}
function currentLocation() {
  var currentLocation = this.$db('currentLocation');
  return JSON.parse(JSON.strignify(currentLocation));
}
</script>
