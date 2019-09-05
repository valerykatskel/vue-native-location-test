<template>
  <view class="container">
    <view class="statusBar"/>
    <status-bar
            background-color="red"
            bar-style="light-content"
        />
    <view class="main">
      <text>Координаты:</text>
      <text>{{message}}</text>
      <text>{{location.latitude}}</text>
      <text>{{location.longitude}}</text>
      <!-- <map-view class="container"
        :initial-region="coordinates"
      /> -->
      <touchable-opacity :on-press="getLocation" >
          <text>get my location</text>
      </touchable-opacity>
    </view>
  </view>
</template>

<script>
import { Constants } from 'expo';
import * as Location from 'expo-location';
import * as Permissions from 'expo-permissions';



export default {
  data: function() {
    return {
      // coordinates: {
      //   latitude: 12.91074,
      //   longitude: 77.5996363,
      //   latitudeDelta: 0.0922,
      //   longitudeDelta: 0.0421
      // },
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
.statusBar {
  backgroundColor: blue;
  width: 100%;
  alignSelf: flex-start; 
  height: 24px;
}
.container {
  background-color: white;
  align-items: center;
  justify-content: center;
  flex: 1;
}
.main {
  flex:1;
}
.text-color-primary {
  color: blue;
}
</style>
