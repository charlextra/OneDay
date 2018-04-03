<template>
  <f7-page>
    <f7-navbar v-if="$theme.material">
      <f7-nav-left>
        <f7-link icon="icon-bars" open-panel="left"></f7-link>
      </f7-nav-left>
      <f7-nav-center sliding>{{$lang('jobs')}}</f7-nav-center>
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
    <f7-block>
      <div class="content-block-title">{{$lang('enregistrerjob')}}</div>
      <form class="list-block inputs-list">
      <ul>
        <li>
          <div class="item-content">
            <div class="item-inner">
              <p :class="{ 'control': true }">
                <div class="item-title label">{{$lang('jobs')}}</div>
                <div class="item-input">
                  <input v-validate="'required'" :class="{'input': true, 'is-danger': errors.has('libservice') }" v-model="jobs.libservice" name="libservice" :data-vv-as="$lang('v_libjob')" type="text" :placeholder="$lang('jobs')"/>
                </div>
                <span v-show="errors.has('libservice')" class="danger">{{ errors.first('libservice') }}</span>
              </p>
            </div>
          </div>
        </li>
        <li>
          <div class="item-content">
            <div class="item-inner">
              <p :class="{ 'control': true }">
                <div class="item-title label">{{$lang('description')}}</div>
                <div class="item-input">
                  <input v-validate="'required'" :class="{'input': true, 'is-danger': errors.has('description') }" v-model="jobs.description" name="description" data-vv-as="La description" type="text" :placeholder="$lang('description')"/>
                </div>
                <span v-show="errors.has('description')" class="danger">{{ errors.first('description') }}</span>
              </p>
            </div>
          </div>
        </li>
        <li>
          <div class="item-content">
            <div class="item-inner">
              <p :class="{ 'control': true }">
                <div class="item-title label">{{$lang('nbrepers')}}</div>
                <div class="item-input">
                  <input v-validate="'required'" :class="{'input': true, 'is-danger': errors.has('nbrepers') }" v-model="jobs.nbrepers" name="nbrepers" data-vv-as="La nbrepers" type="number" :placeholder="$lang('nbrepers')"/>
                </div>
                <span v-show="errors.has('nbrepers')" class="danger">{{ errors.first('nbrepers') }}</span>
              </p>
            </div>
          </div>
        </li>
        <li>
          <div class="item-content">
            <div class="item-inner">
              <p :class="{ 'control': true }">
                <div class="item-title label">{{$lang('datedeb')}}</div>
                <div class="item-input">
                  <input v-validate="'required'" :class="{'input': true, 'is-danger': errors.has('datedeb') }" v-model="jobs.datedeb" name="datedeb" data-vv-as="La datedeb" type="date" :placeholder="$lang('datedeb')"/>
                </div>
                <span v-show="errors.has('datedeb')" class="danger">{{ errors.first('datedeb') }}</span>
              </p>
            </div>
          </div>
        </li>
        <li>
          <div class="item-content">
            <div class="item-inner">
              <p :class="{ 'control': true }">
                <div class="item-title label">{{$lang('datefin')}}</div>
                <div class="item-input">
                  <input v-validate="'required'" :class="{'input': true, 'is-danger': errors.has('datefin') }" v-model="jobs.datefin" name="datefin" data-vv-as="La datefin" type="date" :placeholder="$lang('datefin')"/>
                </div>
                <span v-show="errors.has('datefin')" class="danger">{{ errors.first('datefin') }}</span>
              </p>
            </div>
          </div>
        </li>
      </ul>
      <div v-if="this.jobs.edit==''">
        <f7-button small fill raised bg="orange"  @click.prevent="validateBeforeSubmit" internal>{{$lang('sauvegarder')}}</f7-button>
        <!--<f7-button route-tab-link="#tab-1" href="/tabs/">Button 1</f7-button>-->
      </div>
      <div v-else>
        <f7-button small fill raised bg="green"  @click.prevent="edit()" internal>{{$lang('sauvegarder')}}</f7-button>
        <br />
        <f7-button small fill raised bg="red"  @click.prevent="annuler()" internal>{{$lang('terminer')}}</f7-button>
      <!--<f7-button @click="$db('jobs.description', null)">Remove data</f7-button>-->
      <!--<f7-button small fill raised bg="red" @click="errors.clear" internal>Effacer</f7-button>-->
    </div>
    </form>
  </f7-block>
    <f7-block>
      <f7-list media-list>
      <f7-list-item swipeout v-for="(value, key) in this.$db('jobs')"  :title="value.libservice" :subtitle="value.datedeb+' / '+value.datedeb" :text="value.description" @swipeout:deleted="supprimer(key)">
        <f7-swipeout-actions>
          <f7-swipeout-button close color="blue" @click="SetEdit(key)">{{$lang('editer')}}</f7-swipeout-button>
          <f7-swipeout-button v-show="value.edit==''" delete>{{$lang('supprimer')}}</f7-swipeout-button>
        </f7-swipeout-actions>
      </f7-list-item>
    </f7-list>
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
        }],
        jobs: {
            libservice: '',
            description: '',
            nbrepers: '',
            datedeb: '',
            datefin: '',
            lat: '',
            long: '',
            lat2: '',
            long2: '',
            edit:''
        }
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
        this.jobs.lat = position.coords.latitude;
        this.jobs.long = position.coords.longitude;
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
          this.jobs.lat2 =  results[0].geometry.location.lat();
          this.jobs.long2 = results[0].geometry.location.lng();
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
    validateBeforeSubmit() {
      this.$validator.validateAll().then((result) => {
        if (result) {
          // eslint-disable-next-line
          //this.$db('jobs', this.jobs);
          this.$fireDB('jobs').push(this.jobs);
          this.$f7.alert('enrégistrement effectué','Succès');
          return;
        }
        this.$f7.alert('Les informations du formulaire sont incorrects','Erreur');

      });
    },
    supprimer(key){
      this.$fireDB('jobs').child(key).remove();
    },
    SetEdit(key){
      this.$fireDB('jobs').child(key).update({edit: key});
      this.jobs = JSON.parse(JSON.stringify(this.$db('jobs.'+key)));
    },
    annuler(key){
      this.$fireDB('jobs').child(this.jobs.edit).update({edit: ''});
      this.jobs.edit = '';
      this.$f7.getCurrentView().router.back({pageName:'accueil', force: true})
    },
    edit(){
      this.$fireDB('jobs').child(this.jobs.edit).update(this.jobs);
      this.$fireDB('jobs').child(this.jobs.edit).update({edit: ''});
      this.jobs.edit = '';
      this.$f7.alert('Modification réussie','Succès');
    }
  },
  created() {
    this.$fireDB('currentLocation').on('value', (snap) => {
      this.$db('currentLocation', snap.val())
      this.$db('center', snap.val())
    })
    this.$fireDB('jobs').on('value', (snap) => {
    this.$db('jobs', snap.val())
    })
  }
}
function currentLocation() {
  var currentLocation = this.$db('currentLocation');
  return JSON.parse(JSON.strignify(currentLocation));
}
</script>
