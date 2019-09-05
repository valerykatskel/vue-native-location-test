<template>
  <view class="container">
    
    <text>Координаты:</text>
    <text>{{message}}</text>
    <text>{{location.latitude}}</text>
    <text>{{location.longitude}}</text>

    <touchable-opacity :on-press="getLocation" >
        <text>get my location</text>
    </touchable-opacity>
  </view>
</template>

<script>
import { Constants } from 'expo';
import * as Location from 'expo-location';
import * as Permissions from 'expo-permissions';
import { yandexMap, ymapMarker } from 'vue-yandex-maps'


export default {
  components: { yandexMap, ymapMarker },
  data: function() {
    return {
      mapSettings: {
        apiKey: '',
        lang: 'ru_RU',
        coordorder: 'latlong',
        version: '2.1'
      },
      location: {},
      errorMessage: "",
      message: "...",
      placemarks: [
        {
          coords: [54.8, 39.8],
          properties: {}, // define properties here
          options: {}, // define options here
          clusterName: "1",
          callbacks: { click: function() {} }
        }
      ]
    };
  },

  components: {yandexMap, ymapMarker },

  methods: {
    getLocation: function() {
      let _self = this;
      _self.message = 'Начинаем получать координаты';

        Permissions.askAsync(Permissions.LOCATION).then(({status}) => {
          if (status !== "granted") {
            _self.message = 'Разрешение на доступ к местоположению не получено'
          } else {
            _self.message = 'разрешение на получение координат получено';
          }
         
        Location.getCurrentPositionAsync({}).then(({coords}) => {
          _self.message = `Широта: ${coords.latitude} Долгота: ${coords.longitude}`;
        });
      }).catch((err)=>{
        console.log(err);
        _self.message = 'Ошибка получения координат';
      });
    }
  }
};
</script>

<style>
.container {
  background-color: white;
  align-items: center;
  justify-content: center;
  flex: 1;
}
.text-color-primary {
  color: blue;
}
</style>
