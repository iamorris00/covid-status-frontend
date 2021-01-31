<template>
  <div class="body">
    <div class="container-header  text-white" >
      <div class="col-text">
        <div class="p-custom">
          <h1 class="font-fraunces">Covid19</h1>
          <h1 class="font-fraunces text-white">Estado mundial actual</h1>
        </div>
        <div class="font-p">
          <p class="text-white text-md-left ">Estado actual de casos activos, recuperados, muertes y vacunas del coronavirus
            (COVID-19) un proceso constante en 190 paises en el mundo.</p>
        </div>
      </div>
      <div class="col-6">
        <img  src="@/assets/map.svg" id="map" alt="World to save" />
      </div>
    </div>
    <br/>
    <input v-model="countryFilter" class="form-control combine-margin" placeholder="Buscar por pais">
<!--    <div class="sorter-containers">-->
<!--      <span class="d-block mb-2">Ordenar por:</span>-->
<!--      <span class="pointer-event d-block mb-2">Alfabeticamente</span>-->
<!--    </div>-->
    <br/>
    <ul style="padding: 0">
      <li style="list-style: none;" v-for="data in filteredCountries" :key="data.country">
        <div class="container-countries">
          <div class="country">
            <div class="country-header">
              <img v-bind:src="data.countryFlags.url" class="img-country" alt="Afghanistan"/>
              <span class="text-country">{{data.country}}</span>
              <span class="text-xs"></span>
            </div>
            <div class="country-data-text">
              <span class="text-xs font-weight-bold">Población total: </span>
              <span class="text-xs">{{data.population}}</span>
              <span class="text-xs font-weight-bold">Total de casos confirmados: </span>
              <span class="text-xs">{{data.confirmed}}</span>
              <span class="text-xs font-weight-bold">Total de recuperados: </span>
              <span class="text-xs">{{data.recovered}}</span>
              <span class="text-xs font-weight-bold">Total de muertes: </span>
              <span class="text-xs">{{data.death}}</span>
              <span class="text-xs font-weight-bold">Total de casos activos: </span>
              <span class="text-xs">{{data.active}}</span>
              <span class="text-xs font-weight-bold">Total de Vacunados: </span>
              <span class="text-xs">{{data.vaccinated}}</span>
            </div>
            <div class="load-bar-progress">
              <span style="z-index: 4">{{data.vaccWidth.toString()}}%</span>
              <div class="load-variable-recovered" v-bind:style="{width: data.recoveredWidth + '%'}"></div>
              <div class="load-variable-active" v-bind:style="{width: data.activeWidth + '%'}"></div>
              <div class="load-variable-deaths" v-bind:style="{width: data.deathWidth + '%'}"></div>
              <div class="load-variable-vacc" v-bind:style="{width: data.vaccWidth + '%'}"></div>
            </div>
          </div>
        </div>
      </li>
    </ul>
  </div>
</template>


<script>
// @ is an alias to /src

import axios from "axios";


export default {
  name: 'CovidHome',
  components: {

  },
  data: function (){
    return{
      countries:[],
      populations:[],
      confirmeds:[],
      actives:[],
      recovereds:[],
      deaths:[],
      vaccinateds:[],
      totalPercentages:[],
      recoveredWidths:[],
      activeWidths:[],
      deathWidths:[],
      vaccWidths:[],
      allData:[],
      countryFilter:'',
      images:[{
        url:require('@/assets/contry_flags_svg/afghanistan.svg'),
        alt:'Afghanistan'
      },
        {
          url:require('@/assets/contry_flags_svg/albania.svg'),
          alt:'Albania'
        },
        {
          url:require('@/assets/contry_flags_svg/algeria.svg'),
          alt: 'Algeria'
        },
        {
          url: require('@/assets/contry_flags_svg/andorra.svg'),
          alt: 'Andorra'
        },
        {
          url: require('@/assets/contry_flags_svg/angola.svg'),
          alt: 'Angola'
        },
        {
          url: require('@/assets/contry_flags_svg/antigua and barbuda.svg'),
          alt: 'Antigua and Barbuda'
        },
        {
          url: require('@/assets/contry_flags_svg/argentina.svg'),
          alt: 'Argentina'
        },
        {
          url: require('@/assets/contry_flags_svg/armenia.svg'),
          alt: 'Armenia'
        },
        {
          url: require('@/assets/contry_flags_svg/australia.svg'),
          alt: 'Australia'
        },
        {
          url: require('@/assets/contry_flags_svg/austria.svg'),
          alt: 'Austria'
        },
        {
          url: require('@/assets/contry_flags_svg/azerbaijan.svg'),
          alt: 'Azerbaijan'
        },
        {
          url: require('@/assets/contry_flags_svg/bahamas.svg'),
          alt: 'Bahamas'
        },
        {
          url: require('@/assets/contry_flags_svg/bahrain.svg'),
          alt: 'Bahrain'
        },
        {
          url: require('@/assets/contry_flags_svg/bangladesh.svg'),
          alt: 'Bangladesh'
        },
        {
          url: require('@/assets/contry_flags_svg/barbados.svg'),
          alt: 'Barbados'
        },
        {
          url: require('@/assets/contry_flags_svg/belarus.svg'),
          alt: 'Belarus'
        },
        {
          url: require('@/assets/contry_flags_svg/belgium.svg'),
          alt: 'Belgium'
        },
        {
          url: require('@/assets/contry_flags_svg/belize.svg'),
          alt: 'Belize'
        },
        {
          url: require('@/assets/contry_flags_svg/benin.svg'),
          alt: 'Benin'
        },
        {
          url: require('@/assets/contry_flags_svg/bhutan.svg'),
          alt: 'Bhutan'
        },
        {
          url: require('@/assets/contry_flags_svg/bolivia.svg'),
          alt: 'Bolivia'
        },
        {
          url: require('@/assets/contry_flags_svg/bosnia-and-herzegovina.svg'),
          alt: 'Bosnia y Herzegovina'
        },
        {
          url: require('@/assets/contry_flags_svg/botswana.svg'),
          alt: 'Botswana'
        },
        {
          url: require('@/assets/contry_flags_svg/brazil.svg'),
          alt: 'Brazil'
        },
        {
          url: require('@/assets/contry_flags_svg/brunei.svg'),
          alt: 'Brunei'
        },
        {
          url: require('@/assets/contry_flags_svg/bulgaria.svg'),
          alt: 'Bulgaria'
        },
        {
          url: require('@/assets/contry_flags_svg/burkina-faso.svg'),
          alt: 'Burkina Faso'
        },
        {
          url: require('@/assets/contry_flags_svg/armenia.svg'),
          alt: 'Burma xd'
        },
        {
          url: require('@/assets/contry_flags_svg/burundi.svg'),
          alt: 'Burundi'
        },
        {
          url: require('@/assets/contry_flags_svg/cabo vede.svg'),
          alt: 'Cabo verde'
        },
        {
          url: require('@/assets/contry_flags_svg/cambodia.svg'),
          alt: 'Cambodia'
        },
        {
          url: require('@/assets/contry_flags_svg/cameroon.svg'),
          alt: 'Cameroon'
        },
        {
          url: require('@/assets/contry_flags_svg/canada.svg'),
          alt: 'Canada'
        },
        {
          url: require('@/assets/contry_flags_svg/central african republic.svg'),
          alt: 'Republica Central de Africa'
        },
        {
          url: require('@/assets/contry_flags_svg/chad.svg'),
          alt: 'Chad'
        },
        {
          url: require('@/assets/contry_flags_svg/chile.svg'),
          alt: 'Chile'
        },
        {
          url: require('@/assets/contry_flags_svg/china.svg'),
          alt: 'China'
        },
        {
          url: require('@/assets/contry_flags_svg/colombia.svg'),
          alt: 'Polombia'
        },
        {
          url: require('@/assets/contry_flags_svg/comoros.svg'),
          alt: 'Comoros'
        },
        {
          url: require('@/assets/contry_flags_svg/congo.svg'),
          alt: 'Congo (Brazzaville)'
        },
        {
          url: require('@/assets/contry_flags_svg/congo 2.svg'),
          alt: 'Congo (Kinshasa)'
        },
        {
          url: require('@/assets/contry_flags_svg/costa rica.svg'),
          alt: 'Costa rica'
        },
        {
          url: require('@/assets/contry_flags_svg/armenia.svg'),
          alt: 'Cote dIvoire xd'
        },
        {
          url: require('@/assets/contry_flags_svg/croatia.svg'),
          alt: 'Croatia'
        },
        {
          url: require('@/assets/contry_flags_svg/cuba.svg'),
          alt: 'Cuba'
        },
        {
          url: require('@/assets/contry_flags_svg/cyprus.svg'),
          alt: 'Cyprus'
        },
        {
          url: require('@/assets/contry_flags_svg/czechia.svg'),
          alt: 'Czechia'
        },
        {
          url: require('@/assets/contry_flags_svg/denmark.svg'),
          alt: 'Dinamarca'
        },
        {
          url: require('@/assets/contry_flags_svg/djibouti.svg'),
          alt: 'Djibouti'
        },
        {
          url: require('@/assets/contry_flags_svg/dominica.svg'),
          alt: 'Dominica'
        },
        {
          url: require('@/assets/contry_flags_svg/dominican republic.svg'),
          alt: 'Dominican Republic'
        },
        {
          url: require('@/assets/contry_flags_svg/ecuador.svg'),
          alt: 'Ecuador'
        },
        {
          url: require('@/assets/contry_flags_svg/egypt.svg'),
          alt: 'Egypt'
        },
        {
          url: require('@/assets/contry_flags_svg/cuba.svg'),
          alt: 'El salvadorxd'
        },
        {
          url: require('@/assets/contry_flags_svg/equatorial guinea.svg'),
          alt: 'Guinea ecuatorial'
        },
        {
          url: require('@/assets/contry_flags_svg/eritrea.svg'),
          alt: 'Eritrea'
        },
        {
          url: require('@/assets/contry_flags_svg/estonia.svg'),
          alt: 'Estonia'
        },
        {
          url: require('@/assets/contry_flags_svg/armenia.svg'),
          alt: 'Eswatinixd'
        },
        {
          url: require('@/assets/contry_flags_svg/ethiopia.svg'),
          alt: 'Ethiopia'
        },
        {
          url: require('@/assets/contry_flags_svg/fiji.svg'),
          alt: 'Fiji'
        },
        {
          url: require('@/assets/contry_flags_svg/finland.svg'),
          alt: 'Finland'
        },
        {
          url: require('@/assets/contry_flags_svg/france.svg'),
          alt: 'France'
        },
        {
          url: require('@/assets/contry_flags_svg/gabon.svg'),
          alt: 'Gabon'
        },
        {
          url: require('@/assets/contry_flags_svg/gambia.svg'),
          alt: 'Gambia'
        },
        {
          url: require('@/assets/contry_flags_svg/georgia.svg'),
          alt: 'Georgia'
        },
        {
          url: require('@/assets/contry_flags_svg/germany.svg'),
          alt: 'Germany'
        },
        {
          url: require('@/assets/contry_flags_svg/ghana.svg'),
          alt: 'Ghana'
        },
        {
          url: require('@/assets/contry_flags_svg/greece.svg'),
          alt: 'Greece'
        },
        {
          url: require('@/assets/contry_flags_svg/grenada.svg'),
          alt: 'Grenada'
        },
        {
          url: require('@/assets/contry_flags_svg/guatemala.svg'),
          alt: 'Guatemala'
        },
        {
          url: require('@/assets/contry_flags_svg/guinea.svg'),
          alt: 'Guinea'
        },
        {
          url: require('@/assets/contry_flags_svg/guinea-bissau.svg'),
          alt: 'Guinea-Bissau'
        },
        {
          url: require('@/assets/contry_flags_svg/guyana.svg'),
          alt: 'Guyana'
        },
        {
          url: require('@/assets/contry_flags_svg/haiti.svg'),
          alt: 'Haiti'
        },
        {
          url: require('@/assets/contry_flags_svg/holy see.svg'),
          alt: 'Holy see'
        },
        {
          url: require('@/assets/contry_flags_svg/honduras.svg'),
          alt: 'Honduras'
        },
        {
          url: require('@/assets/contry_flags_svg/hungary.svg'),
          alt: 'Hungary'
        },
        {
          url: require('@/assets/contry_flags_svg/iceland.svg'),
          alt: 'Iceland'
        },
        {
          url: require('@/assets/contry_flags_svg/india.svg'),
          alt: 'India'
        },
        {
          url: require('@/assets/contry_flags_svg/indonesia.svg'),
          alt: 'Indonesia'
        },
        {
          url: require('@/assets/contry_flags_svg/iran.svg'),
          alt: 'Iran'
        },
        {
          url: require('@/assets/contry_flags_svg/iraq.svg'),
          alt: 'Iraq'
        },
        {
          url: require('@/assets/contry_flags_svg/ireland.svg'),
          alt: 'Ireland'
        },
        {
          url: require('@/assets/contry_flags_svg/israel.svg'),
          alt: 'Israel'
        },
        {
          url: require('@/assets/contry_flags_svg/italy.svg'),
          alt: 'Italy'
        },
        {
          url: require('@/assets/contry_flags_svg/jamaica.svg'),
          alt: 'Jamaica'
        },
        {
          url: require('@/assets/contry_flags_svg/japan.svg'),
          alt: 'Japan'
        },
        {
          url: require('@/assets/contry_flags_svg/jordan.svg'),
          alt: 'Jordan'
        },
        {
          url: require('@/assets/contry_flags_svg/kazakhstan.svg'),
          alt: 'Kazakhstan'
        },
        {
          url: require('@/assets/contry_flags_svg/armenia.svg'),
          alt: 'Kenyaxd'
        },
        {
          url: require('@/assets/contry_flags_svg/korea, south.svg'),
          alt: 'Korea, South'
        },
        {
          url: require('@/assets/contry_flags_svg/kosovo.svg'),
          alt: 'Kosovo'
        },
        {
          url: require('@/assets/contry_flags_svg/kuwait.svg'),
          alt: 'Kuwait'
        },
        {
          url: require('@/assets/contry_flags_svg/kyrgyzstan.svg'),
          alt: 'Kyrgyzstan'
        },
        {
          url: require('@/assets/contry_flags_svg/laos.svg'),
          alt: 'Laos'
        },
        {
          url: require('@/assets/contry_flags_svg/latvia.svg'),
          alt: 'Latvia'
        },
        {
          url: require('@/assets/contry_flags_svg/lebanon.svg'),
          alt: 'Lebanon'
        },
        {
          url: require('@/assets/contry_flags_svg/lesotho.svg'),
          alt: 'Lesotho'
        },
        {
          url: require('@/assets/contry_flags_svg/liberia.svg'),
          alt: 'Luberia'
        },
        {
          url: require('@/assets/contry_flags_svg/libya.svg'),
          alt: 'Libya'
        },
        {
          url: require('@/assets/contry_flags_svg/liechtenstein.svg'),
          alt: 'Liechtenstein'
        },
        {
          url: require('@/assets/contry_flags_svg/lithuania.svg'),
          alt: 'Lithuania'
        },
        {
          url: require('@/assets/contry_flags_svg/luxembourg.svg'),
          alt: 'Luxembourg'
        },
        {
          url: require('@/assets/contry_flags_svg/madagascar.svg'),
          alt: 'Madagascar'
        },
        {
          url: require('@/assets/contry_flags_svg/malawi.svg'),
          alt: 'Malawi'
        },
        {
          url: require('@/assets/contry_flags_svg/malaysia.svg'),
          alt: 'Malaysia'
        },
        {
          url: require('@/assets/contry_flags_svg/maldives.svg'),
          alt: 'Maldives'
        },
        {
          url: require('@/assets/contry_flags_svg/mali.svg'),
          alt: 'Mali'
        },
        {
          url: require('@/assets/contry_flags_svg/malta.svg'),
          alt: 'Malta'
        },
        {
          url: require('@/assets/contry_flags_svg/marshall island.svg'),
          alt: 'Marshall Islands'
        },
        {
          url: require('@/assets/contry_flags_svg/mauritania.svg'),
          alt: 'Mauritania'
        },
        {
          url: require('@/assets/contry_flags_svg/mauritius.svg'),
          alt: 'Mauritius'
        },
        {
          url: require('@/assets/contry_flags_svg/mexico.svg'),
          alt: 'Mexico'
        },
        {
          url: require('@/assets/contry_flags_svg/micronesia.svg'),
          alt: 'Micronesia'
        },
        {
          url: require('@/assets/contry_flags_svg/moldova.svg'),
          alt: 'Moldova'
        },
        {
          url: require('@/assets/contry_flags_svg/monaco.svg'),
          alt: 'Monaco'
        },
        {
          url: require('@/assets/contry_flags_svg/mongolia.svg'),
          alt: 'Mongolia'
        },
        {
          url: require('@/assets/contry_flags_svg/montenegro.svg'),
          alt: 'Montenegro'
        },
        {
          url: require('@/assets/contry_flags_svg/morocco.svg'),
          alt: 'Morocco'
        },
        {
          url: require('@/assets/contry_flags_svg/mozambique.svg'),
          alt: 'Mozambique'
        },
        {
          url: require('@/assets/contry_flags_svg/namibia.svg'),
          alt: 'Namibia'
        },
        {
          url: require('@/assets/contry_flags_svg/nepal.svg'),
          alt: 'Nepal'
        },
        {
          url: require('@/assets/contry_flags_svg/netherlands.svg'),
          alt: 'Netherlands'
        },
        {
          url: require('@/assets/contry_flags_svg/new zealand.svg'),
          alt: 'New Zealand'
        },
        {
          url: require('@/assets/contry_flags_svg/nicaragua.svg'),
          alt: 'Nicaragua'
        },
        {
          url: require('@/assets/contry_flags_svg/niger.svg'),
          alt: 'Niger'
        },
        {
          url: require('@/assets/contry_flags_svg/nigeria.svg'),
          alt: 'Nigeria'
        },
        {
          url: require('@/assets/contry_flags_svg/north macedonia.svg'),
          alt: 'North Macedonia'
        },
        {
          url: require('@/assets/contry_flags_svg/norway.svg'),
          alt: 'Norway'
        },
        {
          url: require('@/assets/contry_flags_svg/oman.svg'),
          alt: 'Oman'
        },
        {
          url: require('@/assets/contry_flags_svg/pakistan.svg'),
          alt: 'Pakistan'
        },
        {
          url: require('@/assets/contry_flags_svg/panama.svg'),
          alt: 'Panama'
        },
        {
          url: require('@/assets/contry_flags_svg/papua-new-guinea.svg'),
          alt: 'Papua New Guinea'
        },
        {
          url: require('@/assets/contry_flags_svg/paraguay.svg'),
          alt: 'Paraguay'
        },
        {
          url: require('@/assets/contry_flags_svg/peru.svg'),
          alt: 'Peru'
        },
        {
          url: require('@/assets/contry_flags_svg/philippines.svg'),
          alt: 'Philippines'
        },
        {
          url: require('@/assets/contry_flags_svg/poland.svg'),
          alt: 'Poland'
        },
        {
          url: require('@/assets/contry_flags_svg/portugal.svg'),
          alt: 'Portugal'
        },
        {
          url: require('@/assets/contry_flags_svg/qatar.svg'),
          alt: 'Qatar'
        },
        {
          url: require('@/assets/contry_flags_svg/romania.svg'),
          alt: 'Romania'
        },
        {
          url: require('@/assets/contry_flags_svg/russia.svg'),
          alt: 'Russia'
        },
        {
          url: require('@/assets/contry_flags_svg/rwanda.svg'),
          alt: 'Rwanda'
        },
        {
          url: require('@/assets/contry_flags_svg/saint kitts and nevis.svg'),
          alt: 'Saint Kitts and Nevis'
        },
        {
          url: require('@/assets/contry_flags_svg/saint lucia.svg'),
          alt: 'Saint Lucia'
        },
        {
          url: require('@/assets/contry_flags_svg/saint vicent and grenadines.svg'),
          alt: 'Saint Vicent and the Grenadines'
        },
        {
          url: require('@/assets/contry_flags_svg/samoa.svg'),
          alt: 'Samoa'
        },
        {
          url: require('@/assets/contry_flags_svg/san-marino.svg'),
          alt: 'San Marino'
        },
        {
          url: require('@/assets/contry_flags_svg/sao tome and principe.svg'),
          alt: 'Sao Tome andPrincipe'
        },
        {
          url: require('@/assets/contry_flags_svg/saudi-arabia.svg'),
          alt: 'Saudi Arabia'
        },
        {
          url: require('@/assets/contry_flags_svg/senegal.svg'),
          alt: 'Senegal'
        },
        {
          url: require('@/assets/contry_flags_svg/serbia.svg'),
          alt: 'Serbia'
        },
        {
          url: require('@/assets/contry_flags_svg/seychelles.svg'),
          alt: 'Seychelles'
        },
        {
          url: require('@/assets/contry_flags_svg/sierra leone.svg'),
          alt: 'Sierra Leone'
        },
        {
          url: require('@/assets/contry_flags_svg/singapore.svg'),
          alt: 'Singapore'
        },
        {
          url: require('@/assets/contry_flags_svg/slovakia.svg'),
          alt: 'Slovakia'
        },
        {
          url: require('@/assets/contry_flags_svg/slovenia.svg'),
          alt: 'Slovenia'
        },
        {
          url: require('@/assets/contry_flags_svg/solomon islands.svg'),
          alt: 'Solomon Islands'
        },
        {
          url: require('@/assets/contry_flags_svg/somalia.svg'),
          alt: 'Somalia'
        },
        {
          url: require('@/assets/contry_flags_svg/south africa.svg'),
          alt: 'South Africa'
        },
        {
          url: require('@/assets/contry_flags_svg/south-sudan.svg'),
          alt: 'South Sudan'
        },
        {
          url: require('@/assets/contry_flags_svg/spain.svg'),
          alt: 'Spain'
        },
        {
          url: require('@/assets/contry_flags_svg/sri lanka.svg'),
          alt: 'Sri Lanka'
        },
        {
          url: require('@/assets/contry_flags_svg/sudan.svg'),
          alt: 'Sudan'
        },
        {
          url: require('@/assets/contry_flags_svg/suriname.svg'),
          alt: 'Suriname'
        },
        {
          url: require('@/assets/contry_flags_svg/sweden.svg'),
          alt: 'Sweden'
        },
        {
          url: require('@/assets/contry_flags_svg/switzerland.svg'),
          alt: 'Switzerland'
        },
        {
          url: require('@/assets/contry_flags_svg/syria.svg'),
          alt: 'Syria'
        },
        {
          url: require('@/assets/contry_flags_svg/taiwan.svg'),
          alt: 'Taiwan'
        },
        {
          url: require('@/assets/contry_flags_svg/tajikistan.svg'),
          alt: 'Tajikistan'
        },
        {
          url: require('@/assets/contry_flags_svg/tanzania.svg'),
          alt: 'Tanzania'
        },
        {
          url: require('@/assets/contry_flags_svg/thailand.svg'),
          alt: 'Thailand'
        },
        {
          url: require('@/assets/contry_flags_svg/armenia.svg'),
          alt: 'Timor-Lestxd'
        },
        {
          url: require('@/assets/contry_flags_svg/togo.svg'),
          alt: 'Togo'
        },
        {
          url: require('@/assets/contry_flags_svg/trinidad and tobago.svg'),
          alt: 'Trinidad and Tobago'
        },
        {
          url: require('@/assets/contry_flags_svg/tunisia.svg'),
          alt: 'Tunisia'
        },
        {
          url: require('@/assets/contry_flags_svg/turkey.svg'),
          alt: 'Turkey'
        },
        {
          url: require('@/assets/contry_flags_svg/us.svg'),
          alt: 'US'
        },
        {
          url: require('@/assets/contry_flags_svg/uganda.svg'),
          alt: 'Uganda'
        },
        {
          url: require('@/assets/contry_flags_svg/ukraine.svg'),
          alt: 'Ukraine'
        },
        {
          url: require('@/assets/contry_flags_svg/united arab emirates.svg'),
          alt: 'United Arab Emirates'
        },
        {
          url: require('@/assets/contry_flags_svg/united kingdom.svg'),
          alt: 'United Kingdom'
        },
        {
          url: require('@/assets/contry_flags_svg/uruguay.svg'),
          alt: 'Uruguay'
        },
        {
          url: require('@/assets/contry_flags_svg/uzbekistn.svg'),
          alt: 'Uzbekistan'
        },
        {
          url: require('@/assets/contry_flags_svg/vanuatu.svg'),
          alt: 'Vanuatu'
        },
        {
          url: require('@/assets/contry_flags_svg/venezuela.svg'),
          alt: 'Venezuela'
        },
        {
          url: require('@/assets/contry_flags_svg/vietnam.svg'),
          alt: 'Vietnam'
        },
        {
          url: require('@/assets/contry_flags_svg/armenia.svg'),
          alt: 'West Bank and Gaza xd'
        },
        {
          url: require('@/assets/contry_flags_svg/yemen.svg'),
          alt: 'Yemen'
        },
        {
          url: require('@/assets/contry_flags_svg/zambia.svg'),
          alt: 'Zambia'
        },
        {
          url: require('@/assets/contry_flags_svg/zimbabwe.svg'),
          alt: 'Zimbabwe'
        },


      ]
    }
  },
  methods: {
    numberWithCommas(x) {
  return x.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",");
  },
    roundToXDigits(value, digits) {
      if(!digits){
        digits = 2;
      }
      value = value * Math.pow(10, digits);
      value = Math.round(value);
      value = value / Math.pow(10, digits);
      return value;
    }
  },
  mounted() {
    axios.get("http://34.205.74.186/api/status/").then(data =>{
      for (let i = 0; i < data.data.length; i++) {
        this.countries.push(data.data[i].country_region)
        this.populations.push(this.numberWithCommas(data.data[i].population))
        this.confirmeds.push(this.numberWithCommas(data.data[i].confirmed))
        this.actives.push(this.numberWithCommas(data.data[i].active))
        this.recovereds.push(this.numberWithCommas(data.data[i].recovered))
        this.deaths.push(this.numberWithCommas(data.data[i].death))
        this.vaccinateds.push(this.numberWithCommas(data.data[i].vaccinated))
        this.recoveredWidths.push(this.roundToXDigits(data.data[i].recovered/data.data[i].population*100))
        this.activeWidths.push(this.roundToXDigits(this.recoveredWidths[i]+data.data[i].active/data.data[i].population*100))
        this.deathWidths.push(this.roundToXDigits(this.activeWidths[i]+data.data[i].death/data.data[i].population*100))
        this.vaccWidths.push(this.roundToXDigits(this.deathWidths[i]+data.data[i].vaccinated/data.data[i].population*100))
        let result = {
          country: this.countries[i],
          population: this.populations[i],
          confirmed: this.confirmeds[i],
          active: this.actives[i],
          recovered: this.recovereds[i],
          death: this.deaths[i],
          vaccinated: this.vaccinateds[i],
          recoveredWidth: this.recoveredWidths[i],
          activeWidth:this.activeWidths[i],
          deathWidth: this.deathWidths[i],
          vaccWidth: this.vaccWidths[i],
          countryFlags: this.images[i],
        }
        this.allData.push(result)
      }
      console.log(this.allData)
      console.log(this.allData.countryFlags.url[0])
    })
  },
  computed:{
      filteredCountries: function(){
      return this.allData.filter((data) =>{
      return data.country.toLowerCase().match(this.countryFilter.toLowerCase());
    })
    }
  }
}
</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Fraunces:wght@100;600&display=swap');

.container-header{
  width: 100%;
  padding: 0rem 1rem 0rem;
  background-color: #05555B;
  display: flex;
  flex-wrap: wrap;
  margin-bottom: -15px;
}

.p-custom{
  padding: 4rem 0.5rem 1rem;
  font-size: 20px;
}
.col-text{
  flex: 0 0 50%;
  max-width: 50%;
  position: relative;
  width: 100%;
  padding-right: 15px;
  padding-left: 15px;
  padding-top: 30px;
}

.font-fraunces{
  font-family:'Fraunces', serif;
  font-weight: bold;
  color: antiquewhite;
}

.body{
  background-color: #E5E7EB;

}

.font-p{
  font-family: Calibri;
  font-size: 20px;

}

.container-countries{
  background-color: white;
  margin: 5px;
  border-radius: 20px;
  padding: 2.5rem;
}

.combine-margin{
  margin: -30px auto 0;
  max-width: 50%;
  position: relative;
  z-index: 1;
}

#map{
  place-self: center;
  height: 100%;
  max-width: 100%;
}


.country{
  margin-bottom: 3.5rem;
}

.img-country{
  width: 2rem;
  margin-right: 0.75rem;
  height: auto;
}

.country-header{
  margin-bottom: 0.75rem;
  align-items: center;
  display: flex;
}

.text-xs{
  padding-left: 5px;
  font-size: 0.75rem;
  font-weight: 400;
}

.text-country{
  font-width: 700;
}

.country-data-text{
  align-items: center;
  display: flex;
  max-width: 100%;
}
@media screen and (max-width:675px) {
  img#map {
    display: none;
  }
  .col-text{
    max-width: 100%;
    flex: none;
  }
  .p-custom{
    padding: 2rem 0.5rem 1rem;
    font-size: 20px;
  }
  .col-6{
    max-width: 0;
    display: none;
    padding: 0rem;
  }
  .country-data-text{
    display: grid;
  }
}

.load-bar-progress{
  width: 100%;
  position: relative;
  overflow: hidden;
  margin-top: 1rem;
  height: 2.5rem;
  justify-content: center;
  align-items: center;
  display: flex;
  border-radius: 0.4rem;
  background-color: lightgray;
}

.load-variable-recovered{
  left: 0;
  top: 0;
  background-color: #35cb5d;
  position: absolute;
  z-index: 3;
  height: 100%;
}

.load-variable-active{
  left: 0;
  top: 0;
  background-color: yellow;
  position: absolute;
  z-index: 2;
  height: 100%;
}

.load-variable-deaths{
  left: 0;
  top: 0;
  background-color: crimson;
  position: absolute;
  z-index: 1;
  height: 100%;
}

.load-variable-vacc{
  left: 0;
  top: 0;
  background-color: #226eab;
  position: absolute;
  z-index: 0;
  height: 100%;
}

//.sorter-containers{
//  flex-direction: row;
//  color: gray;
//  opacity: 100%;
//  margin-top: 1.5rem;
//  font-size: .875rem;
//  line-height: 1.25rem;
//  align-items: center;
//  display: flex;
//  width: 80%;
//  justify-content: space-evenly;
//}
</style>
