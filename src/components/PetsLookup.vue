<template>
<div role="region" aria-live="polite" aria-relevant="all">

              <!--LOADING ICON START -->

              <section v-if="loading" class="loading-container">
                <span class="sr-only">Loading...</span>
                <span class="paws top-right fa fa-paw fa-3x"></span>
                <span class="paws top-left fa fa-paw fa-3x"></span>
                <span class="paws bottom-right fa fa-paw fa-3x"></span>
                <span class="paws bottom-left fa fa-paw fa-3x"></span>

              </section>

              <!--LOADING ICON END -->

              <div class="fadeIn" v-cloak>

                <section id="errorMessage" v-if="errored">
                  <div class="alert alert-danger" role="alert">
                    <p class="h4"><span class="fa fa-exclamation-circle"></span> We're sorry, we're not able to retrieve this information at the moment, please try back later.</p>

                  </div>
                </section>

                <!-- CARD LIST START -->

                <!--CARD ROW CONTAINER START -->
                <div v-if="!loading && !errored">

                  <!-- FILTER START -->

                  <div class="row">
                    <div class="col-md-4"><label for="postSearch">Search by Name</label> <input type="text" id="postSearch" class="form-control" v-model="search" /></div>

                    <div class="col-md-4">
                      <label for="typeSelect">Animal Type</label>
                      <select id="typeSelect" class="form-control" v-model="selectedType">
                        <option value="">All</option>
                        <option value="cat">Cat</option>
                        <option value="dog">Dog</option>
                        <option value="bird">Bird</option>
                        <option value="livestock">Livestock</option>
                        <option value="other">Other (rodents, rabbits, reptiles, etc.)</option>

                      </select>

                    </div>
                    <div class="col-md-4"> <label for="idSearch">Search by ID </label> <input type="text" id="idSearch" class="form-control" v-model="searchId" /></div>
                    <div class="col-sm-12 mt-2">
                      <p>
                        <a data-toggle="collapse" href="#refine-results" role="button" aria-expanded="false" aria-controls="refine-results">
                          <span class="fas fa-filter" aria-hidden="true"></span> Refine results
                        </a>
                      </p>
                      <div class="collapse" id="refine-results">
                        <div class="card card-body">

                          <div class="card-body">
                            <div class="row">

                              <div class="col-md-6">

                                <!-- GENDER START -->
                                <label for="sexSelect">Sex</label>
                                <select id="sexSelect" class="form-control" v-model="selectedSex">
                                  <option value="">All</option>
                                  <option value="female">Female</option>
                                  <option value="male">Male</option>
                                </select>
                                <!-- GENDER END -->

                                <!-- AGE START -->
                                <label for="ageSelect">Age</label>
                                <select multiple id="ageSelect" class="form-control" v-model="selectedAge">
                                  <option value="">All</option>
                                  <option value="less1">Younger than 1</option>
                                  <option value="1to3">1 to 3</option>
                                  <option value="4to6">4 to 6</option>
                                  <option value="7to9">7 to 9</option>
                                  <option value="seniors">10 and older</option>
                                  <option value="NO AGE">No age given</option>
                                </select>
                                <!-- AGE END -->
                                <!-- LOCATION START -->
                                <label for="locationSelect">Location</label>
                                <select id="locationSelect" class="form-control" v-model="selectedLocation">
                                  <option value="">All</option>
                                  <option value="raskc">King County Pet Adoption Center</option>
                                  <option value="foster">Foster home</option>
                                  <option value="reber">Reber Ranch</option>

                                  <optgroup label="Petco locations">
                                    <option value="covington">Covington</option>
                                    <option value="kirkland">Kirkland</option>
                                    <option value="tukwila">Tukwila</option>
                                  </optgroup>
                                  <!--
                 <option v-for="item in info_location" v-bind:value="item || 'undefined'">{{item}}</option>-->
                                </select>
                                <!-- LOCATION END -->
                              </div>
                              <div class="col-md-6">

                                <!-- TEMPERAMENT START -->
                                <label for="tempSelect">Temperament</label>
                                <ul id="tempSelect" class="list-unstyled">
                                  <li><label><input name="temp" type="radio" v-model="selectedTemp" value=""> All</label></li>
                                  <li><label><input name="temp" type="radio" v-model="selectedTemp" value="GREEN"> <img style="height:34px" src="https://kingcounty.gov/~/media/depts/regional-animal-services/images/All-Images/green.ashx"> <span>Easy-going Green</span></label></li>
                                  <li><label><input name="temp" type="radio" v-model="selectedTemp" value="RED"> <img class="center-block" style="height:34px" src="https://kingcounty.gov//~/media/depts/regional-animal-services/images/All-Images/red.ashx"> <span>Rambuncious Red</span></label></li>
                                  <li><label><input name="temp" type="radio" v-model="selectedTemp" value="BLUE"> <img style="height:34px" src="https://kingcounty.gov//~/media/depts/regional-animal-services/images/All-Images/blue.ashx"> <span>Bashful Blue</span></label></li>
                                </ul>
                                <!-- TEMPERAMENT END -->

                              </div>

                            </div>
                            <div class="card-body">
                              <div class="row">
                                <button id="resultsButton" class="btn btn-link" type="button" data-toggle="collapse" data-target="#refine-results" aria-expanded="false" aria-controls="refine-results">See my selections</button>
                                <button class="btn btn-xs btn-link m-t m-r" v-on:click="resetForm">Clear all filters</button>
                              </div>
                            </div>
                          </div>

                        </div>
                      </div>

                    </div>

                  </div>
                  
                  <ul class="list-inline m-t-md">
                    <li v-if="search" class="list-inline-item"><button class="btn btn-xs btn-default" v-on:click="search = ''">Search term: {{this.search}} <span class="fa fa fa-times-circle" aria-hidden="true"></span></button></li>
                    <li v-if="searchId" class="list-inline-item"><button class="btn btn-xs btn-default" v-on:click="searchId = ''">ID Number: {{this.searchId}} <span class="fa fa fa-times-circle" aria-hidden="true"></span></button></li>

                    <li v-if="selectedType" class="list-inline-item"><button class="btn btn-xs btn-default" v-on:click="selectedType = ''">{{this.selectedType}} <span class="fa fa-times-circle" aria-hidden="true"></span></button></li>
                    <li v-if="selectedLocation" class="list-inline-item"><button class="btn btn-xs btn-default" v-on:click="selectedLocation = ''">Location: {{this.selectedLocation}} <span class="fa fa fa-times-circle" aria-hidden="true"></span></button></li>
                    <li v-if="selectedTemp" class="list-inline-item"><button class="btn btn-xs btn-default" v-on:click="selectedTemp = ''">Temperament: {{this.selectedTemp}} <span class="fa fa fa-times-circle" aria-hidden="true"></span></button></li>
                    <li v-if="selectedAge.length > 0" class="list-inline-item"><button class="btn btn-xs btn-default" v-on:click="selectedAge = []; ageClear()">Age <span class="fa fa fa-times-circle" aria-hidden="true"></span></button></li>
                    <li v-if="selectedSex" class="list-inline-item"><button class="btn btn-xs btn-default" v-on:click="selectedSex = ''">{{this.selectedSex}} <span class="fa fa fa-times-circle" aria-hidden="true"></span></button></li>
                    <li v-if="itemsCount < lessThan" class="list-inline-item"><button class="btn btn-xs btn-link" v-on:click="resetForm">Clear all filters <span class="fa fa-times-circle" aria-hidden="true"></span></button></li>
                  </ul>
                   
                  <p class="text-center lead p-3" v-if="!loading && !errored && !itemsCount < 1">

                    Results: <strong>{{itemsCount}}</strong>

                  </p>

                  <!-- FILTER END -->

                  <div class="row results" v-bind:key="i" v-for="i in Math.ceil(computed_items.length / 3)">

                    <div v-bind:key="i" v-for="(items, i) in computed_items.slice((i - 1) * 3, i * 3)" v-bind:class="{ 'col-md-4': computed_items.length > 1, 'col-lg-4 offset-lg-4 col-sm-12': computed_items.length <= 1 }">

                      <div class="card mb-3 h-100">

                        <img class="card-img-top pet-image" v-bind:alt="items.animal_name + ' the ' + items.animal_type" v-lazy="items.image.url" loading="lazy">

                        <div class="card-body pb-1">
                          <h3 v-bind:id="items.animal_name" class="card-title float-left">{{items.animal_name || items.animal_id}}</h3>
                          <img class="float-right color-icon" :src="'https://kingcounty.gov/~/media/depts/regional-animal-services/images/All-Images/'+items.temperament" alt="">

                          <span class="sr-only">{{items.temperament }}</span>
                        </div>
                        <div class="card-body pt-0">
                          <ul class="list-unstyled">
                            <li>{{items.age | titlecase}} old</li>
                            <li>{{ items.animal_type }}</li>
                            <li>{{items.animal_gender | removeSpayed }} {{items.animal_breed | lowercase }}</li>
                            <li>Received on: {{items.date | dateformat}}</li>
                          </ul>
                          <button class="btn btn-link btn-block stretched-link" data-toggle="modal" v-bind:data-target="'#'+items.animal_id">Meet {{items.animal_name || "me"}}</button>
                        </div>
                      </div>

                      <!-- Modal -->
                      <div class="modal fade" v-bind:id="items.animal_id" tabindex="-1" role="dialog" v-bind:aria-labelledby="'label'+items.animal_id">
                        <div class="modal-dialog modal-lg" role="document">
                          <div class="modal-content">
                            <div class="modal-header">
                              <h4 class="modal-title float-left" v-bind:id="'label'+items.animal_id">{{items.animal_name || items.animal_id }}
                                <img class="color-icon" :src="'https://kingcounty.gov/~/media/depts/regional-animal-services/images/All-Images/'+items.temperament" alt="">

                                <span class="sr-only">{{items.temperament }}</span>

                              </h4>

                              <button type="button" class="btn btn-default float-right" data-dismiss="modal">Close</button>

                            </div>
                            <div class="modal-body">
                              <div id="modal-image-container" class="col-xs-12 col-sm-6 float-right" style="margin-bottom: 20px;">
                                <img v-bind:alt="items.animal_name + ' the ' + items.animal_type" :src="items.image.url" class="img-fluid center-block">
                              </div>

                              <div class="p-r" v-html="items.memo">

                              </div>

                              <p class="box p-3">For more information, please reference Animal ID Number <strong>{{ items.animal_id }} </strong>and call <strong>206-296-3936</strong>.</p>

                            </div>
                            <div class="modal-footer">
                              <button type="button" class="btn btn-default" data-dismiss="modal">Close</button>

                            </div>
                          </div>
                        </div>
                      </div>

                    </div>

                    <!-- NO RESULTS MESSAGE START -->

                    <!-- NO RESULTS MESSAGE END -->

                  </div>
                  <!--STUFF GOES IN HERE-->

                  <section class="row" id="noResults" v-if="!loading && !computed_items.length && !errored">
                    <div class="col alert" role="alert">
                      <p class="h1 text-danger">Hmm...</p>
                      <p>We tried to sniff out animals matching your search, but didn't find any. A different search or filter combination might find more results!</p>

                      <div class="text-center"><img class="img-fluid" src="https://kingcounty.gov/~/media/depts/regional-animal-services/images/banners/dog-search-results.ashx" alt=""></div>

                    </div>
                  </section>

                  <!-- CARD ROW CONTAINER END -->
                </div>
              </div>
              <!--CLOAK -->
            </div>
</template>

<script>
import axios from 'axios';
import Vue from 'vue'
import VueLazyload from 'vue-lazyload'
 
Vue.use(VueLazyload)

export default {
  name: 'PetsLookup', 
  components: {

  },

    data() {
      return {
        info: [],
        loading: true,
        errored: false,
        search: "",
        searchId: "",
        selectedType: "",
        selectedId: "",
        selectedTemp: "",
        selectedAge: [],
        selectedLocation: "",
        selectedSex: ""
      };
    },

    updated() {
     
    },
    mounted() {
      axios
        .get(
          "https://data.kingcounty.gov/resource/yaai-7frk.json?&record_type=ADOPTABLE&$order=date%20ASC"
        )

        .then(response => {
          this.info = response.data;

          //console.log(response);
        })
        .catch(error => {
          console.log(error);
          this.errored = true;
        })

        .finally(() => (this.loading = false));

      
    },

    computed: {
      itemsCount() {
        return this.computed_items.length;
      },
      lessThan() {
        return this.info.length;
      },

      computed_items: function() {
        let filterSearch = this.search 
        let filterId = this.searchId 
        let filterType = this.selectedType 
        let filterLocation = this.selectedLocation 
        let filterTemp = this.selectedTemp 
        let filterAge = this.selectedAge 
        let filterSex = this.selectedSex 
        

        return this.info.filter(function(item) {
          //SEARCH FUNCTION

          let filtered = true;

          if (filtered) {

             if (filterSearch.length > 1) {

              if (item.animal_name != undefined) {
                filtered = 
                  item.animal_name
                  .toLowerCase()
                  .includes(filterSearch.toLowerCase())
              } if (item.animal_name === undefined) {
                return false
              }
            }
          }
          if (filtered) {
            if (filterId.length > 1) {

              if (item.animal_id != undefined) {
                filtered = 
                  item.animal_id
                  .toUpperCase()
                  .includes(filterId.toUpperCase())
              } if (item.animal_id === undefined) {
                return false
              }

            }
          }          
          if (filtered) {
            if (filterTemp && filterTemp.length > 0) {
              if (item.temperament != undefined) {
                filtered = item.temperament == filterTemp;
              }
              if (item.temperament === undefined) {
                return false;
              }
              filtered = item.temperament == filterTemp;
            }
          }

          if (filtered) {
            if (filterType && filterType.length > 0) {
              if (item.animal_type != undefined) {
                if (filterType == "") {
                  filtered = item.animal_type == filterType;
                } else {
                  if (
                    item.animal_type
                      .toString()
                      .toLowerCase()
                      .includes("cat")
                  ) {
                    var cat = "cat";
                    filtered = cat == filterType;
                  }
                  if (
                    item.animal_type
                      .toString()
                      .toLowerCase()
                      .includes("dog")
                  ) {
                    var dog = "dog";
                    filtered = dog == filterType;
                  }
                  if (
                    item.animal_type
                      .toString()
                      .toLowerCase()
                      .includes("bird")
                  ) {
                    var bird = "bird";
                    filtered = bird == filterType;
                  }
                  if (
                    item.animal_type
                      .toString()
                      .toLowerCase()
                      .includes("livestock")
                  ) {
                    var livestock = "livestock";
                    filtered = livestock == filterType;
                  } 
                  if (
                    !item.animal_type.toString().toLowerCase().includes(cat) &&
                    !item.animal_type.toString().toLowerCase().includes(dog) &&
                    !item.animal_type.toString().toLowerCase().includes(bird) &&
                    !item.animal_type.toString().toLowerCase().includes(livestock)
                  ) {
                    var other = "other";
                    filtered = other == filterType;
                  }                  
                }
              }
              if (item.animal_gender === undefined) {
                return false;
              }
            }
          }

          if (filtered) {
             if (filterLocation && filterLocation.length > 0) {
              if (item.current_location != undefined) {
                if (filterLocation == "") {
                  filtered = item.current_location == filterLocation;
                } else {
                  if (
                    item.current_location
                      .toString()
                      .toLowerCase()
                      .includes("foster home")
                  ) {
                    var foster = "foster";
                    filtered = foster == filterLocation;
                  }
                  if (
                    item.current_location
                      .toString()
                      .toLowerCase()
                      .includes("reber")
                  ) {
                    var reber = "reber";
                    filtered = reber == filterLocation;
                  }
                  if (
                    item.current_location
                      .toString()
                      .toLowerCase()
                      .includes("king county pet adoption center")
                  ) {
                    var raskc = "raskc";
                    filtered = raskc == filterLocation;
                  }
                  if (
                    item.current_location
                      .toString()
                      .toLowerCase()
                      .includes("tukwila")
                  ) {
                    var tukwila = "tukwila";
                    filtered = tukwila == filterLocation;
                  } 
                  if (
                    item.current_location
                      .toString()
                      .toLowerCase()
                      .includes("covington")
                  ) {
                    var covington = "covington";
                    filtered = covington == filterLocation;
                  } 
                                    if (
                    item.current_location
                      .toString()
                      .toLowerCase()
                      .includes("kirkland petco")
                  ) {
                    var kirkland = "kirkland";
                    filtered = kirkland == filterLocation;
                  } 
                }
              }
              if (item.current_location === undefined) {
                return false;
              }

          }
          }

          if (filtered) {
            if (filterAge && filterAge.length > 0) {
              if (item.age != undefined) {
                if (filterAge == "") {
                  filtered = !item.age == filterAge;
                } else {
                  if (
                    item.age.includes("YEAR") &&
                    !item.age.includes("NO AGE") &&
                    (item.age.charAt(0).includes("1") ||
                      item.age.charAt(0).includes("2") ||
                      item.age.charAt(0).includes("3")) &&
                    Number.isNaN(parseInt(item.age.charAt(1)))
                  ) {
                    var age1 = "1to3";
                    filtered = filterAge.includes(age1);
                  }
                  if (
                    item.age.includes("YEAR") &&
                    !item.age.includes("NO AGE") &&
                    (item.age.charAt(0).includes("4") ||
                      item.age.charAt(0).includes("5") ||
                      item.age.charAt(0).includes("6")) &&
                    Number.isNaN(parseInt(item.age.charAt(1)))
                  ) {
                    var age2 = "4to6";
                    filtered = filterAge.includes(age1 || age2);
                  }
                  if (
                    item.age.includes("YEAR") &&
                    !item.age.includes("NO AGE") &&
                    (item.age.charAt(0).includes("7") ||
                      item.age.charAt(0).includes("8") ||
                      item.age.charAt(0).includes("9")) &&
                    Number.isNaN(parseInt(item.age.charAt(1)))
                  ) {
                    var age3 = "7to9";
                    filtered = filterAge.includes(age1 || age2 || age3);
                  }
                  if (
                    item.age.includes("YEAR") &&
                    !item.age.includes("NO AGE") &&
                    Number.isNaN(parseInt(item.age.charAt(1))) === false
                  ) {
                    var age4 = "seniors";
                    filtered = filterAge.includes(age1 || age2 || age3 || age4);
                  }
                  if (
                    item.age.includes("WEEK") ||
                    (item.age.includes("MONTH") &&
                      !item.age.includes("NO AGE") &&
                      !item.age.includes("YEAR"))
                  ) {
                    var age5 = "less1";
                    filtered = filterAge.includes(
                      age1 || age2 || age3 || age4 || age5
                    );
                  }
                  if (item.age.includes("NO AGE")) {
                    var age6 = "NO AGE";
                    filtered = filterAge.includes(
                      age1 || age2 || age3 || age4 || age5 || age6
                    );
                  }
                }
              }
              if (item.age === undefined) {
                return false;
              }
            }
          }

          if (filtered) {
            if (filterSex && filterSex.length > 0) {
              if (item.animal_gender != undefined) {
                if (filterSex == "") {
                  filtered = item.animal_gender == filterSex;
                } else {
                  if (
                    item.animal_gender
                      .toString()
                      .toLowerCase()
                      .includes("female")
                  ) {
                    var female = "female";
                    filtered = female == filterSex;
                  }
                  if (
                    item.animal_gender
                      .toString()
                      .toLowerCase()
                      .includes("neutered") ||
                    item.animal_gender
                      .toString()
                      .toLowerCase()
                      .startsWith("m")
                  ) {
                    var male = "male";
                    filtered = male == filterSex;
                  }
                }
              }
              if (item.animal_gender === undefined) {
                return false;
              }
              //filtered = item.animal_gender == filterSex;
            }
          }

          return filtered;
        });
      },

      info_type: function() {
        return [
          
          ...new Set(
            this.info.map(i => i.animal_type).slice().sort()
             )

        ];
      },

      info_location: function() {
        return [
          ...new Set(
            this.info
              .map(i => i.current_location)
              .slice()
              .sort()
          )
        ];
      },
    },

    methods: {

      resetForm: function(e) {
        e.preventDefault();
        this.search = "";
        this.searchId = "";
        this.selectedType = "";
        this.selectedAge = [];
        this.selectedTemp = "";
        this.selectedLocation = "";
        this.selectedSex = "";
        this.ageClear();
      },
      ageClear: function() {
        var ageSelect = document.getElementById("ageSelect").options;
        for (var i = 0; i < ageSelect.length; i++) {
          ageSelect[i].selected = false;
        }
      }
    },

    filters: {
      titlecase: function(value) {
        if (!value) return "";
        value = value.toString().toLowerCase();

        return value.charAt(0).toUpperCase() + value.substring(1);
      },
      lowercase: function(value) {
        if (!value) return "";
        value = value.toString().toLowerCase();

        return value;
      },
      dateformat: function(value) {
        var date = new Date(value);
        var formatdate = new Intl.DateTimeFormat("en-US").format(date);
        return formatdate;
      },
      removeSpayed: function(value) {
        var removed = value.charAt(0).toUpperCase() + value.substring(1);

        if (
          value
            .toString()
            .toLowerCase()
            .includes("spayed")
        ) {
          return removed.replace("Spayed", "");
        }
        if (
          value
            .toString()
            .toLowerCase()
            .includes("neutered")
        ) {
          return removed.replace("Neutered", "");
        } else {
          return value;
        }
      }
    }
  }


</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
