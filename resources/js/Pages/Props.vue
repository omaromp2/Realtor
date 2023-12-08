<template>
  <app-layout>
    <template #header>
      <h2 class="font-semibold text-xl text-gray-800 leading-tight">Properties</h2>

      <label for="operation">Buy or Rent</label>
      <select v-model="selOp" name="operation" @change="valueChange" class="rounded-md">
        <option v-for="op in operation" :key="op" :value="op">{{ op }}</option>
      </select>
      <label for="town">Town</label>
      <select v-model="selPue" name="town" @change="valueChange" class="rounded-md">
        <option v-for="pueblo in pueblos" :key="pueblo" :value="pueblo">{{ pueblo }}</option>
      </select>
    </template>
    <div class="py-12">
      <div v-if="properties.length == 0">
        <center>
          <svg
            xmlns="http://www.w3.org/2000/svg"
            class="h-20 w-20 animate-spin content-center"
            fill="none"
            viewBox="0 0 24 24"
            stroke="currentColor"
            stroke-width="2"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              d="M10.325 4.317c.426-1.756 2.924-1.756 3.35 0a1.724 1.724 0 002.573 1.066c1.543-.94 3.31.826 2.37 2.37a1.724 1.724 0 001.065 2.572c1.756.426 1.756 2.924 0 3.35a1.724 1.724 0 00-1.066 2.573c.94 1.543-.826 3.31-2.37 2.37a1.724 1.724 0 00-2.572 1.065c-.426 1.756-2.924 1.756-3.35 0a1.724 1.724 0 00-2.573-1.066c-1.543.94-3.31-.826-2.37-2.37a1.724 1.724 0 00-1.065-2.572c-1.756-.426-1.756-2.924 0-3.35a1.724 1.724 0 001.066-2.573c-.94-1.543.826-3.31 2.37-2.37.996.608 2.296.07 2.572-1.065z"
            />
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              d="M15 12a3 3 0 11-6 0 3 3 0 016 0z"
            />
          </svg>
        </center>
      </div>

      <div
        class="max-w-7xl mx-auto sm:px-6 lg:px-8 grid grid-cols-1 gap-4 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4"
      >
        <div v-for="prop in randomList" :key="prop" class="grid-cols-4 flex">
          <HouseCard class="h-full w-full">
            <template #header>
              <h2
                v-if="prop.title.length <= 20"
                class="font-semibold text-xl text-gray-800 leading-tight"
              >{{ prop.title }}</h2>

              <h2
                v-else-if="prop.title.length >= 30"
                class="font-bold text-sm text-gray-800 leading-tight"
              >{{ prop.title }}</h2>

              <h2 v-else class="font-bold text-base text-gray-800 leading-tight">{{ prop.title }}</h2>

              <div class="bg-indigo-600 rounded-md">
                <div class="grid grid-cols-2 text-gray-200" v-if="prop.src == 'deshow'">
                  <div>
                    <i class="fa-solid fa-door-open"></i>
                    {{ prop.rooms }} rooms
                  </div>
                  <div>
                    <i class="fa-solid fa-bath"></i>
                    {{ prop.baths }} baths
                  </div>
                  <div>
                    <i class="fa-solid fa-ruler-combined"></i>
                    {{ prop.dim }} sq/ft
                  </div>
                  <div>
                    <i class="fa-solid fa-tag"></i>
                    {{ prop.webId }}
                  </div>
                </div>
                <div v-else class="grid grid-cols-2 text-gray-200">
                  <!-- {{ prop.specs }} -->
                  <div>
                    <i class="fa-solid fa-door-open"></i>
                    {{ prop.rooms }} rooms
                  </div>
                  <div>
                    <i class="fa-solid fa-bath"></i>
                    {{ prop.baths }} baths
                  </div>
                  <div>
                    <i class="fa-solid fa-ruler-combined"></i> N/A
                  </div>
                  <div>
                    <i class="fa-solid fa-tag"></i> N/A
                  </div>
                </div>
              </div>
            </template>
            <template #image>
              <img
                v-if="prop.src == 'deshow'"
                class="h-8 ml-auto absolute z-10"
                src="https://deshow.com/wp-content/themes/realty-child/images/deshow_logo.svg"
                alt="deshow"
              />
              <img
                v-else-if="prop.src == 'zillow'"
                class="mx-auto absolute z-10 w-36"
                src="https://s.zillowstatic.com/pfs/static/z-logo-default.svg"
                alt="zellow"
              />
              <img
                v-else
                class="mx-auto absolute z-10 w-36"
                src="https://imgcache.clasificadosonline.com/UDClasMedia/Arte03/LogoClas.gif"
                alt="clas"
              />

              <img
                v-if="prop.src == 'deshow'"
                class="object-cover w-full h-48 mt-2 image"
                :style="prop.img"
              />
              <img
                v-else
                class="object-cover w-full h-48 mt-2 image"
                :src="prop.img"
                :style="prop.img"
              />
            </template>
            <template #content>
              <h1 class="text-lg font-bold text-black">{{ prop.price }}</h1>

              <a :href="prop.url" target="_blank" class="ml-auto">
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  class="h-6 w-6"
                  fill="none"
                  viewBox="0 0 24 24"
                  stroke="currentColor"
                  stroke-width="2"
                >
                  <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    d="M15 12a3 3 0 11-6 0 3 3 0 016 0z"
                  />
                  <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    d="M2.458 12C3.732 7.943 7.523 5 12 5c4.478 0 8.268 2.943 9.542 7-1.274 4.057-5.064 7-9.542 7-4.477 0-8.268-2.943-9.542-7z"
                  />
                </svg>
              </a>
            </template>
          </HouseCard>
        </div>
      </div>
    </div>
  </app-layout>
</template>
<script>
import AppLayout from '@/Layouts/AppLayout'
import cheerio from 'cheerio'
import HouseCard from '../Jetstream/HouseCard.vue'
import axios from 'axios'

export default {
  components: {
    AppLayout,
    HouseCard
  },
  data() {
    return {
      properties: [],
      deshow: [],
      clasificados: [],
      zillow: [],
      operation: [
        'Buy',
        'Rent',
      ],
      selOp: 'Buy',

      pueblos: [
        'All',
        'Adjuntas',
        'Aguada',
        'Aguadilla',
        'Aguas Buenas',
        'Aibonito',
        'Añasco',
        'Arecibo',
        'Arroyo',
        'Barceloneta',
        'Barranquitas',
        'Bayamón',
        'Cabo Rojo',
        'Caguas',
        'Camuy',
        'Canóvanas',
        'Carolina',
        'Cataño',
        'Cayey',
        'Ceiba',
        'Ciales',
        'Cidra',
        'Coamo',
        'Comerío',
        'Corozal',
        'Culebra',
        'Dorado',
        'Fajardo',
        'Florida',
        'Guánica',
        'Guayama',
        'Guayanilla',
        'Guaynabo',
        'Gurabo',
        'Hatillo',
        'Hormigueros',
        'Humacao',
        'Isabela',
        'Jayuya',
        'Juana Díaz',
        'Juncos',
        'Lajas',
        'Lares',
        'Las Marías',
        'Las Piedras',
        'Loíza',
        'Luquillo',
        'Manatí',
        'Maricao',
        'Maunabo',
        'Mayagüez',
        'Moca',
        'Morovis',
        'Naguabo',
        'Naranjito',
        'Orocovis',
        'Patillas',
        'Peñuelas',
        'Ponce',
        'Quebradillas',
        'Rincón',
        'Río Grande',
        'Sabana Grande',
        'Salinas',
        'San Germán',
        'San Juan',
        'San Lorenzo',
        'San Sebastián',
        'Santa Isabel',
        'Toa Alta',
        'Toa Baja',
        'Trujillo Alto',
        'Utuado',
        'Vega Alta',
        'Vega Baja',
        'Vieques',
        'Villalba',
        'Yabucoa',
        'Yauco',
      ],

      selPue: 'All',

    }
  },
  methods: {
    getProperties() {

      // operation type
      let oper = '';

      if (this.selOp == 'Rent') {
        oper = 'rent-to-own';
      } else {
        oper = 'en-venta';
      }

      // pueblo
      let pue = '';
      if (this.selPue == null) {
        pue = all;
      } else {
        pue = this.selPue;
      }

      pue = pue.replace(/\s/g, '-');


      const deShow =
        'https://deshow.com/advance-search/?operation=' + oper +
        '&type=all&subtipo=all&location=' + pue + '&price=&status=all&keyword=&bedrooms=&bathrooms';


      axios(deShow)
        .then(response => {
          const html = response.data;
          const $ = cheerio.load(html);
          const props = [];

          $('.card', html)
            .each(function () {
              const title = $(this).find('H3').text();
              const url = $(this).find('a').attr('href');
              const img = $(this).find('.image').attr('style');
              const specs = $(this).find('.properties').find('span').text();
              // formateamos el texto
              const formated = specs.replace(/Dormitorios|Dormitorio|Baños|baño|sq ft/gi,
                ' ');
              const spit = formated.split(' ');
              const rooms = spit[0];
              const baths = spit[2];
              const dim = spit[4];
              const webId = spit[6];


              let price = $(this).find('.price').find('span').text();
              // price = price.replace(/\$|renta mensual/gi, '');
              // price = price.replace(/renta mensual| 2$/gi, '');
              const src = 'deshow';

              props.push({
                src,
                title,
                url,
                img,
                specs,
                price,
                rooms,
                baths,
                dim,
                webId
              });

              // console.log('props', props);

            });

          this.deshow = props;

          // this.properties = props;

          for (const key in this.deshow) {
            if (Object.hasOwnProperty.call(this.deshow, key)) {
              const element = this.deshow[key];
              this.properties.push(element);
            }
          }


        })
        .catch(error => {
          console.log(error)
        });
    },

    getClasificados() {

      // pueblo
      let pue = '';
      if (this.selPue == null || this.selPue == 'All') {
        pue = '';
      } else {
        pue = this.selPue;
      }

      pue = pue.replace(/\s/g, '+');

      console.log('pue', pue);


      let url = '';

      if (this.selOp == 'Rent') {
        url =
          'https://www.clasificadosonline.com/UDRentalsListingAdv.asp?RentalsPueblos=' + pue + '%25&Category=%25&Bedrooms=%25&LowPrice=0&HighPrice=9999999999999999&Area=&IncPrecio=1&redirecturl=%2FUDRentalsListingAdvMap.asp&BtnSearchListing=Listado';
      } else {
        url =
          'https://www.clasificadosonline.com/UDREListing.asp?RESPueblos=' + pue + '&Category=%25&LowPrice=0&HighPrice=999999999&Bedrooms=%25&Area=&Repo=Repo&Opt=Opt&BtnSearchListing=Ver+Listado&redirecturl=%2Fudrelistingmap.asp&IncPrecio=1';
      }

      // llamamos el url
      axios(url)
        .then(response => {
          const html = response.data;
          const $ = cheerio.load(html);
          const props = [];

          $('.dv-classified-row', html)
            .each(function () {
              const title = $(this).find('.link-blue-color').text();
              const url = 'https://www.clasificadosonline.com' + $(this).find('a').attr(
                'href');
              const img = $(this).find('img').attr('src');
              const specs = $(this).find('span:contains("Cuartos")').text().trim();
              const price = $(this).find('.Tahoma16BrownNound').find('font').text();
              const src = 'clasificados';

              // formateamos el texto
              const formated = specs.replace(/Cuartos|Baños|\n|\t/gi, '');
              // console.log('orig', specs);

              // console.log('formated', formated);

              const spit = formated.split(' ');
              const rooms = spit[0];
              const baths = spit[2];


              props.push({
                src,
                title,
                url,
                img,
                specs,
                price,
                rooms,
                baths
              });

            });

          this.clasificados = props;

          for (const key in this.clasificados) {
            if (Object.hasOwnProperty.call(this.clasificados, key)) {
              const element = this.clasificados[key];
              this.properties.push(element);
            }
          }

          // this.properties.concat(props);
          // this.properties = props;

          // console.log('clasificados', this.clasificados);
        })
        .catch(error => {
          console.log(error)
        });
    },

    getZillow() {

      let oper = '';

      if (this.selOp == 'Rent') {
        oper = 'for_rent';
      } else {
        oper = 'for_sale';
      }

      // pueblo
      let pue = '';
      if (this.selPue == null || this.selPue == 'All') {
        pue = 'puerto-rico_rb';
      } else {
        pue = this.selPue;
        pue = pue.replace(/\s/g, '-');
        pue = pue + ',-PR_rb';
      }


      const url = 'https://www.zillow.com/homes/' + oper + '/' + pue + '/';
      // const url = 'https://www.zillow.com/pr/?searchQueryState=%7B%22pagination%22%3A%7B%7D%2C%22usersSearchTerm%22%3A%22PR%22%2C%22mapBounds%22%3A%7B%22west%22%3A-67.693246140625%2C%22east%22%3A-65.474007859375%2C%22south%22%3A17.131092782349988%2C%22north%22%3A19.26267829208483%7D%2C%22regionSelection%22%3A%5B%7B%22regionId%22%3A48%2C%22regionType%22%3A2%7D%5D%2C%22isMapVisible%22%3Atrue%2C%22filterState%22%3A%7B%22sort%22%3A%7B%22value%22%3A%22globalrelevanceex%22%7D%2C%22ah%22%3A%7B%22value%22%3Atrue%7D%7D%2C%22isListVisible%22%3Atrue%2C%22mapZoom%22%3A9%7D';
      // llamamos el url

      console.log('url', url);


      axios(url)
        .then(response => {
          const html = response.data;
          const $ = cheerio.load(html);
          const props = [];

          $('.list-card', html)
            .each(function () {
              const title = $(this).find('.list-card-addr').text();
              const url = $(this).find('a.list-card-link').attr('href');
              const img = $(this).find('a.list-card-img>img').attr('src');
              const price = $(this).find('div.list-card-price').text();
              const specs = $(this).find('ul.list-card-details').text();
              const src = 'zillow';

              // formateamos el texto
              const formated = specs.replace(/bds|ba|sqft-/gi, ' ');
              const spit = formated.split(' ');
              const rooms = spit[0];
              const baths = spit[2];
              const dim = spit[4];

              props.push({
                src,
                title,
                url,
                img,
                specs,
                price,
                rooms,
                baths,
                dim
              });

            });

          this.zillow = props;

          // pasamos los valores a Props
          for (const key in this.zillow) {
            if (Object.hasOwnProperty.call(this.zillow, key)) {
              const element = this.zillow[key];
              this.properties.push(element);
            }
          }

        });
    },

    merger() {
      // primero llamamos ambas funciones para halar la data
      this.getProperties();
      this.getClasificados();
      this.getZillow();

    },

    valueChange() {
      // alert(this.selOp);
      this.properties = [];
      this.randomList = [];
      this.merger();
    }

  },
  mounted() {
    this.merger();
  },

  computed: {
    randomList: function () {
      return this.properties.sort(function () {
        return 0.5 - Math.random()
      });
    }
  }

}

</script>
<style scoped>
/* css style for component */
.image {
  height: 180px;
  background-position: center;
  background-size: cover;
  background-repeat: no-repeat;
  position: relative;
}
</style>
